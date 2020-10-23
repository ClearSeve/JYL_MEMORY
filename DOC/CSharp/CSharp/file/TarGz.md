# TarGz

```
using System.IO;
using System.IO.Compression;


//写入压缩流
FileStream fileStream = new FileStream(filename, FileMode.Create, FileAccess.Write);
GZipStream compresssionStream = new GZipStream(fileStream, CompressionMode.Compress);

//写入字符串
StreamWriter writer = new StreamWriter(compresssionStream);
writer.Write(data);
writer.Close();




//读取压缩流
FileStream fileStream = new FileStream(filename, FileMode.Open, FileAccess.Read);
GZipStream compressionstream = new GZipStream(fileStream, CompressionMode.Decompress);

//读取字符串
StreamReader reader = new StreamReader(compressionstream);
string data = reader.ReadToEnd();

//读取字节流
byte[] dat = new byte[512];
while(true)
{
     int len = br.Read(dat, 0, 512);
     if (0 == len) break;
}

```



```
struct tar_header
{
	char name[100];//* 文件/文件夹路径 （文件夹以/结尾）
	char mode[8];//0000777
	char uid[8];//0000000
	char gid[8];//0000000
	char size[12];//* 文件大小【8进制】
	char mtime[12];//修改时间【8进制】
	char chksum[8];//* 除去checksum字段（8字节）其他所有的504个字节的ascii码相加的值再加上256  【8进制】

	char typeflag;//* '0'
/*
#define  lf_oldnormal '\0'       /* normal disk file, unix compatible */
#define  lf_normal    '0'        /* normal disk file */
#define  lf_link      '1'        /* link to previously dumped file */
#define  lf_symlink   '2'        /* symbolic link */
#define  lf_chr       '3'        /* character special file */
#define  lf_blk       '4'        /* block special file */
#define  lf_dir       '5'        /* directory */
#define  lf_fifo      '6'        /* fifo special file */
#define  lf_contig    '7'        /* contiguous file */
*/

	char linkname[100];//""
	char magic[6];//*【ustar 】
	char version[2];//""
	char uname[32];
	char gname[32];
	char devmajor[8];//""
	char devminor[8];//""
	char prefix[155];//""
	char padding[12];//""
};//文件内容以512字节为一个block进行分割，最后一个block不足部分以0补齐
```

```
class TarFileInfo
{
    public string fullPathName;//dir无全路径
    public string relationPathName;
}
class TarOpt
{
    byte[] GenTarHead(string name, int size)
    {
        name = name.Replace("\\", "/");
        if (0 == size) if (name[name.Length - 1] != '/') name += "/";
        byte[] Buf = new byte[512];
        Set(Buf, name, 0, 100);
        Set(Buf, "0000777", 100, 8);
        string si = Convert.ToString(size, 8);
        Set(Buf, si, 124, 12);
        Set(Buf, "ustar ", 257, 100);
        if (size == 0) Buf[156] = 53;// '5';
        int chksum = 256;
        for (int i = 0; i < 512; ++i)
        {
            chksum += Buf[i];
        }
        string chk = Convert.ToString(chksum, 8);
        Set(Buf, chk, 148, 8);
        return Buf;
    }
    void Set(byte[] Buf, string str, int pos, int maxlen)
    {
        byte[] by = Encoding.UTF8.GetBytes(str);
        int len = by.Length > maxlen ? maxlen : by.Length;
        Array.Copy(by, 0, Buf, pos, len);
    }
    void AddDir(BinaryWriter bw, string dirName)
    {
        byte[] Buf = GenTarHead(dirName, 0);
        bw.Write(Buf);
    }
    void AddFile(BinaryWriter bw, TarFileInfo info)
    {
        FileStream fs = new FileStream(info.fullPathName, FileMode.Open);
        int len = (int)fs.Length;
        byte[] Buf = GenTarHead(info.relationPathName, len);
        bw.Write(Buf);
        for (int i = 0; i < len; i += 512)
        {
            byte[] by = new byte[512];
            fs.Read(by, 0, 512);
            bw.Write(by);
        }
    }
    public void CreateTarGz(string dstPathName, List<TarFileInfo> fi)
    {
        FileStream fileStream = new FileStream(dstPathName, FileMode.Create, FileAccess.Write);
        GZipStream compresssionStream = new GZipStream(fileStream, CompressionMode.Compress);
        BinaryWriter bw = new BinaryWriter(compresssionStream);
        foreach (var item in fi)
        {
            if (item.fullPathName == "") AddDir(bw, item.relationPathName);
            else AddFile(bw, item);
        }
        bw.Close();
        compresssionStream.Close();
        fileStream.Close();
    }
    public void ProcessDir(string dir)
    {
        DirectoryInfo TheFolder = new DirectoryInfo(dir);
        List<TarFileInfo> fi = new List<TarFileInfo>();
        int pos = dir.LastIndexOf("\\");
        if (pos == dir.Length - 1) dir = dir.Substring(0, pos);
        string baseDir = dir;
        pos = baseDir.LastIndexOf("\\");
        baseDir = baseDir.Substring(0, pos + 1);
        AddPath(baseDir, dir, fi);
        string dist = dir + ".tar.gz";
        CreateTarGz(dist, fi);
    }
    void AddPath(string baseDir, string path, List<TarFileInfo> PathNameList)
    {
        TarFileInfo tfi = new TarFileInfo();
        tfi.fullPathName = "";
        tfi.relationPathName = path.Substring(baseDir.Length);
        PathNameList.Add(tfi);
        DirectoryInfo TheFolder = new DirectoryInfo(path);
        foreach (FileInfo NextFile in TheFolder.GetFiles("*.*"))
        {
            tfi = new TarFileInfo();
            tfi.fullPathName = NextFile.FullName;
            tfi.relationPathName = tfi.fullPathName.Substring(baseDir.Length);
            PathNameList.Add(tfi);
        }
        foreach (DirectoryInfo NextFolder in TheFolder.GetDirectories())
        {
            AddPath(baseDir, NextFolder.FullName, PathNameList);
        }
    }
    public void ExtractGz(string baseDir,MemoryStream ms)
    {
        if (baseDir[baseDir.Length - 1] != '\\') baseDir += "\\";
        GZipStream compressionstream = new GZipStream(ms, CompressionMode.Decompress);
        BinaryReader br = new BinaryReader(compressionstream);
        while (true)
        {
            try
            {
                if (!ProcPerHead(baseDir, br)) return;
            }
            catch (Exception) { }
        }
    }
    public void ExtractGz(string gzfilepathname)
    {
        string baseDir = gzfilepathname.Substring(0,gzfilepathname.LastIndexOf("\\") + 1);
        FileStream fileStream = new FileStream(gzfilepathname, FileMode.Open, FileAccess.Read);
        GZipStream compressionstream = new GZipStream(fileStream, CompressionMode.Decompress);
        BinaryReader br = new BinaryReader(compressionstream);
        while (true)
        {
            try
            {
                if (!ProcPerHead(baseDir, br)) return;
            }
            catch (Exception) { }
        }
    }
    string getStr(byte[] dat,int pos ,int len)
    {
        int count = 0;
        for(int i = 0; i < len;++i)
        {
            if (dat[pos + i] == 0) break;
            ++count;
        }
        return Encoding.UTF8.GetString(dat, pos, count);
    }
    bool ProcPerHead(string baseDir,BinaryReader br)
    {
        byte[] dat = new byte[512];
        int len = br.Read(dat, 0, 512);
        if (0 == len) return false;
        string name = getStr(dat, 0, 100);
        name = name.Replace("/","\\");
        string size = getStr(dat, 124, 12);
        int si = Convert.ToInt32(size, 8);
        string pathname = baseDir + name;
        if (si == 0)
        {
            Directory.CreateDirectory(pathname);
        }
        else
        {
            int count = si / 512;
            int reduce = si % 512;
            FileStream fs = new FileStream(pathname,FileMode.Create);
            for(int i = 0; i < count;++i)
            {
                int read = br.Read(dat, 0, 512);
                if (0 == read) return false;
                fs.Write(dat, 0, dat.Length);
            }
            if(0 != reduce)
            {
                int read = br.Read(dat, 0, 512);
                if (0 == read) return false;
                fs.Write(dat, 0, reduce);
            }
            fs.Close();
        }
        return true;
    }
}
```