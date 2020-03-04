# c++Mix

## java调用c++
```
    <dependency>
       <groupId>net.java.dev.jna</groupId>
       <artifactId>jna</artifactId>
       <version>4.5.0</version>
    </dependency>
```

```
public class App 
{
    public interface Dll extends Library{		
		Dll instance = (Dll)Native.loadLibrary("D:\\DLT.dll", Dll.class);
		public int  DLT_FUN(String pathName, int val);
	}

    public static void main( String[] args )
    {
        System.setProperty("jna.encoding", "GBK"); //不一定好使
        Dll.instance.DLT_FUN("abc", 10);
    }
}
```

## C++调用java

需要将jre\bin\server\jvm.dll加入环境变量path，移动dll放入工程运行时出错  
加载包时，需要包的全路径，如env->FindClass("java/lang/String");  

### Java文件 

编译成class文件TestFun.class  
```
public class TestFun {	
	public static int intMethod(int x,int y) {  
        return x*y;  
    }
	public void sayHelloFromJava(String str,int listLen,int[] listVal)
	{
	   System.out.println(str);
	   for(int i = 0 ; i <listLen;++i)
	   {
	     System.out.print(listVal[i] + " ");
	   } 
	}
}
```

### C++调用

头文件目录  
C:\Java\jdk1.8.0_60\include;C:\Java\jdk1.8.0_60\include\win32  
Lib目录  
C:\Java\jdk1.8.0_60\lib  
运行dll目录  
C:\Java\jre1.8.0_60\bin\server  


```
#include <jni.h>
#pragma comment(lib,"jvm.lib")

	//加载jvm
	JavaVMInitArgs vm_args;
	memset(&vm_args, 0, sizeof(vm_args));
	vm_args.version = JNI_VERSION_1_8;//jdk版本
	vm_args.nOptions = 1;
	JavaVMOption options[1];
	options[0].optionString = "-Djava.class.path=.";
	vm_args.options = options;

	JNIEnv *env;
	JavaVM *jvm;
	long status = JNI_CreateJavaVM(&jvm, (void**)&env, &vm_args);
	if (status == JNI_ERR) throw(string("JNI_CreateJavaVM erro"));

	//加载类TestFun
	jclass cls = env->FindClass("TestFun");
	if (0 ==  cls ) throw(string("FindClass erro"));


	//调用类中的静态方法intMethod
	jmethodID mid = env->GetStaticMethodID(cls, "intMethod", "(II)I");//返回值int，两个参数为int类型
	if (0 == mid) throw(string("intMethod erro"));
	jint square = env->CallStaticIntMethod(cls, mid, 5,5);
	printf("Result of intMethod: %d\n", square);


	//创建类并调用函数sayHelloFromJava
	jmethodID ctor = env->GetMethodID(cls, "<init>", "()V");
	jobject obj = env->NewObject(cls, ctor);
	mid = env->GetMethodID(cls, "sayHelloFromJava", "(Ljava/lang/String;I[I)V");//无返回值，第一个参数int，第二个String
	if (0 == mid) throw(string("sayHelloFromJava erro"));

	jstring str1 = env->NewStringUTF("I am class Instance");
	const jint len = 3;  
	jintArray testIntArray = env->NewIntArray(len);
	jint test[len] = {1,2,3};
	env->SetIntArrayRegion(testIntArray, 0, len, test);
	env->CallVoidMethod(obj, mid, str1, len, testIntArray);


	jvm->DestroyJavaVM();
```

|域描述符  |                                 |
|:-:      | :-:                             |
|V	      | void                            |
|Z	      | boolean                         |
|B	      | byte                            |
|C	      | char                            |
|S	      | short                           |
|I	      | int                             |
|J	      | long                            |
|F	      | float                           |
|D	      | double                          |
|L..	  | 引用类型 如Ljava/lang/String;    |
|[..	  | 数组类型 如 [I                   |
|[[..	  | 二维数组  如 [[I                 |
