# GPS

## 度       
ddd.ddddd°  
[116.16726]   

## 度.分 
ddd°mm.mmm’ 
[116.10.036’]  //116.10036
GPGGA (WGS-84)

//转度  
116 + 10.036/60 = 116.16726

## 度.分.秒 ：
ddd°mm’ss’’  
[116°10’2’’]  //116.1002  

//转度
116 + 10/60 + 2/3600 = 116.16722

经度1度 ≈ 111.13km   
经度1分 ≈ 1.852km   
经度1秒 ≈ 30.9m   

纬度1度 ≈ 111.31km    
纬度1分 ≈ 1.855km    
纬度1秒 ≈ 30.9m   


## 转换
```
    class GpsThrans
    {
        double transformLon(double x, double y)
        {
            double ret = 300.0 + x + 2.0 * y + 0.1 * x * x + 0.1 * x * y + 0.1 * Math.Sqrt(Math.Abs(x));
            ret += (20.0 * Math.Sin(6.0 * x * Math.PI) + 20.0 * Math.Sin(2.0 * x * Math.PI)) * 2.0 / 3.0;
            ret += (20.0 * Math.Sin(x * Math.PI) + 40.0 * Math.Sin(x / 3.0 * Math.PI)) * 2.0 / 3.0;
            ret += (150.0 * Math.Sin(x / 12.0 * Math.PI) + 300.0 * Math.Sin(x / 30.0 * Math.PI)) * 2.0 / 3.0;
            return ret;
        }
        double transformLat(double x, double y)
        {
            double ret = -100.0 + 2.0 * x + 3.0 * y + 0.2 * y * y + 0.1 * x * y + 0.2 * Math.Sqrt(Math.Abs(x));
            ret += (20.0 * Math.Sin(6.0 * x * Math.PI) + 20.0 * Math.Sin(2.0 * x * Math.PI)) * 2.0 / 3.0;
            ret += (20.0 * Math.Sin(y * Math.PI) + 40.0 * Math.Sin(y / 3.0 * Math.PI)) * 2.0 / 3.0;
            ret += (160.0 * Math.Sin(y / 12.0 * Math.PI) + 320 * Math.Sin(y * Math.PI / 30.0)) * 2.0 / 3.0;
            return ret;
        }
        bool outOfChina(double lat, double lon)
        {
            if (lon < 72.004 || lon > 137.8347) return true;
            if (lat < 0.8293 || lat > 55.8271) return true;
            return false;
        }
        public bool transformToGCJ02FromDu(ref double wgLat, ref double wgLon)
        {//度 -》火星坐标系
            double a = 6378245.0;
            double ee = 0.00669342162296594323;
            if (outOfChina(wgLat, wgLon)) return false;

            double dLat = transformLat(wgLon - 105.0, wgLat - 35.0);
            double dLon = transformLon(wgLon - 105.0, wgLat - 35.0);
            double radLat = wgLat / 180.0 * Math.PI;
            double magic = Math.Sin(radLat);
            magic = 1 - ee * magic * magic;
            double sqrtMagic = Math.Sqrt(magic);
            dLat = (dLat * 180.0) / ((a * (1 - ee)) / (magic * sqrtMagic) * Math.PI);
            dLon = (dLon * 180.0) / (a / sqrtMagic * Math.Cos(radLat) * Math.PI);
            wgLat = wgLat + dLat;
            wgLon = wgLon + dLon;
            return true;
        }


        public double ChangeDuMinToDu(double org)
        {//度分-》度
            int d = (int)org;
            double minu =  (org - d) * 100;
            return d + minu / 60;
        }
        public double ChangeDuMinSecToDu(double org)
        {//度分秒-》度
            int d = (int)org;
            double minu = ((org - d) * 100);
            double sec =  (minu - (int)minu) * 100;
            return d + ((int)minu) / 60.0 + ((int)sec) / 3600.0;
        }
    }
```