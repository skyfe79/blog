---
layout: post
title:  "최신 안드로이드 NDK 튜토리얼"
permalink: /digital-video-tech
date:   2017-12-04 09:14:00 +0900
categories: android
---

`[출처]` : [https://medium.com/@jrejaud/modern-android-ndk-tutorial-630bc11829a2](https://medium.com/@jrejaud/modern-android-ndk-tutorial-630bc11829a2)

이 글에서는 Android Studio2.2이상에서 Android NDK를 사용하여 C 또는 C++ 코드를 사용하는 기본 내용을 다룹니다. 제가 NDK를 시작했을 때 어려움을 겪었기에 다양한 곳에서 배운 정보들을 이 글 하나로 요약했습니다.(접근 가능한 정보들은 글 하단에 링크로 적어 두었습니다.)

이 글은 C 보다는 C++에 좀 더 중점을 두어 작성했습니다.

 * 시작하기(NDK 개발환경 구성하기)
 * 자바에서 호출 할 수 있는 네이티브 메서드 만들기
 * 자바 객체를 자바와 네이티브 코드간에 주고 받기
 * 자바 객체 배열을 자바와 네이티브 코드간에 주고 받기

Android Studio2.2이상은 예저에 쓰이던 ndk-build 대신에 CMake 빌드 스크립트를 사용합니다. NDK를 공부하다가 막히면 [구글의 NDK 샘플 코드](https://github.com/googlesamples/android-ndk)를 참고하시길 바랍니다.

## 시작하기

먼저 구글에서 제공하는 `NDK 시작하기`문서 일독을 추천합니다.

 * [https://developer.android.com/ndk/guides/index.html](https://developer.android.com/ndk/guides/index.html)

구글 가이드는 안드로이드 NDK를 사용하는데 필요한 핵심 정보를 제공합니다.

### C 또는 C++ 소스파일을 프로젝트에 추가하기

 * [https://developer.android.com/studio/projects/add-native-code.html#create-sources](https://developer.android.com/studio/projects/add-native-code.html#create-sources)

### CMake 빌드 스크립트 만들기

 * [https://developer.android.com/studio/projects/add-native-code.html#create-cmake-script](https://developer.android.com/studio/projects/add-native-code.html#create-cmake-script)

다음은 CMakeLists 파일 예제입니다. 작성한 코드에서는 Myfile.cpp인 C++파일과 자바 코드를 연결하려고 합니다.

```cmake
# Sets the minimum version of CMake required to build your native library.
# This ensures that a certain set of CMake features is available to
# your build.

cmake_minimum_required(VERSION 3.4.1)

# Specifies a library name, specifies whether the library is STATIC or
# SHARED, and provides relative paths to the source code. You can
# define multiple libraries by adding multiple add.library() commands,
# and CMake builds them for you. When you build your app, Gradle
# automatically packages shared libraries with your APK.

add_library( # Specifies the name of the library.
             Myfile

             # Sets the library as a shared library.
             SHARED

             # Provides a relative path to your source file(s).
             src/main/cpp/Myfile.cpp )

# Add the directories where the Cpp header files are to let CMake find them during compile time
include_directories(src/main/cpp/)
```

Myfile.cpp에서는 [OpenGL 수학](http://glm.g-truc.net/0.9.8/index.html)과 관련된 라이브러리를 사용하고 있습니다. 필요한 glm 파일들을 다운로드합니다. 받은 glm파일들을 `src/main/cpp/glm`으로 복사합니다. 그리고 `include_directories(src/main/cpp/)`로 glm라이브러리를 포함해야 한다고 알려줍니다.

### Gradle에 네이티브 라이브러리 연결하기

 * [https://developer.android.com/studio/projects/add-native-code.html#link-gradle](https://developer.android.com/studio/projects/add-native-code.html#link-gradle)

CMake 파일에 옵션 인자와 플래그를 지정할 수 있습니다. 예를 들어, Myfile.cpp는 C++11을 필요로 하며 예외를 허용해야할 때 아래처럼 Gradle에 작성해 줍니다.

```gradle
externalNativeBuild {
    cmake {
        // Required by MyFile.cpp
        cppFlags "-std=c++11" //Enable C++ 11
        cppFlags "-fexceptions" //Allow exceptions
    }
```

## 자바와 네이티브 코드간에 통신하기

지금까지 C/C++ 네이티브 코드를 가져와 NDK를 설정했습니다. 그리고 Gradle을 통해 안드로이드 프로젝트에 연결했습니다. 다음은 자바와 네이티브 코드간에 서로 통신할 수 있도록 해줘야 합니다.

### 자바에서는

개인적으로 정적으로 네이티브 라이브러리를 로드하고 호출하며, 네이티브 메서드만을 선언해 놓은 자바 클래스를 만드는 것이 좋다고 생각합니다. 

```java
// Wrapper for native library 
public class NativeLib {      
    
    static {
        // Replace "Myfile" with the name of your Native File
        System.loadLibrary("Myfile");     
    }
    
    // Declare your native methods here     
    public static native String string();
}
```

`System.loadLibrary("Myfile");`을 사용해 `Myfile`을 로드합니다.

### 네이티브에서는

#### 1. 자바에서 호출할 수 있는 네이티브 메서드 만들기

자바 코드에서 다이렉트로 네이티브 메서드를 호출할 수 없습니다. 대신 특정한 방법으로 자바 코드에서 네이티브 메서드가 호출되도록 추가 구현이 필요합니다.

```cpp
//You need this to allow methods that can be invoked by Java
#include <jni.h>

//This syntax is needed if you are invoking a C++ method. C methods are slightly different

//Since the String method returns a String object, specify jstring. Replace this with whatever return object your method has.

extern "C" JNIEXPORT jstring JNICALL
//Replace the crazy ass method name with the path to the Java file that is invoking it (com/example/NativeLib in this case).

//The Native Methods must always have JNIEnv *env, jobject object as the first two parameters
Java_com_example_NativeLib_string(JNIEnv *env, jobject object) {
    //This is how you return a jstring via C++ code
    return env->NewStringUTF("Hello from JNI LIBS!");
}
```

##### C문법 vs C++문법

`출처`: [Original Post by Philippe Leefsma](http://adndevblog.typepad.com/cloud_and_mobile/2013/08/android-ndk-passing-complex-data-to-jni.html)

```cpp
// C syntax: my package name is com.autodesk.and.jnitester
// and my activity invoking the native method is MainActivity,
// hence the name of my method is
//Java_com_autodesk_and_jnitester_MainActivity_MethodName
JNIEXPORT
jstring
JNICALL
Java_com_autodesk_adn_jnitester_MainActivity_getMessageFromNative(
    JNIEnv *env,
    jobject callingObject)
{
    return (*env)->NewStringUTF(env, "Native code rules!");
}


// C++ syntax: Required to declare as extern "C" to prevent c++ compiler
// to mangle function names
extern "C"
{
     JNIEXPORT
     jstring
     JNICALL
     Java_com_autodesk_adn_jnitester_MainActivity_getMessageFromNative(
                JNIEnv *env,
                jobject callingObject)
     {
           return env->NewStringUTF("Native code rules!");
     }
};
```

#### 2. 자바와 네이티브 코드 간에 객체 주고 받기

`출처`: [Original Post by Philippe Leefsma](http://adndevblog.typepad.com/cloud_and_mobile/2013/08/android-ndk-passing-complex-data-to-jni.html)


##### 먼저, 자바쪽에 간단한 POJO를 정의합니다.

```java
public class MeshData
{
    private int _facetCount;
   
    public float[] VertexCoords;
 
    public MeshData(int facetCount)
    {
        _facetCount = facetCount;
       
        VertexCoords = new float[facetCount];
       
        // fills up coords with dummy values
        for(int i=0; i<facetCount; ++i)
        {
           VertexCoords[i] = 10.0f * i;
        }
    }
 
    public int getFacetCount()
    {
        return _facetCount;
    }
}
```

##### 만든 POJO객체를 네이티브 코드로 전달하는 자바 메서드를 작성합니다.

```java
public native float getMemberFieldFromNative(MeshData obj);
```

##### 네이티브 코드에서 전달된 자바 객체의 필드를 읽습니다.

```cpp
JNIEXPORT
jfloat
JNICALL
Java_com_autodesk_adn_jnitester_MainActivity_getMemberFieldFromNative
(
	JNIEnv *env,
	jobject callingObject,
	jobject obj //obj is the MeshData java object passed
) 
{
   float result = 0.0f;
 
   //Get the passed object's class  
   jclass cls = env->GetObjectClass(obj);
 
   // get field [F = Array of float
   jfieldID fieldId = env->GetFieldID(cls, "VertexCoords", "[F");
 
   // Get the object field, returns JObject (because it’s an Array)
   jobject objArray = env->GetObjectField (obj, fieldId);
 
   // Cast it to a jfloatarray
   jfloatArray* fArray = reinterpret_cast<jfloatArray*>(&objArray);
 
   jsize len = env->GetArrayLength(*fArray);
 
   // Get the elements
   float* data = env->GetFloatArrayElements(*fArray, 0);
 
   for(int i=0; i<len; ++i)
   {
        result += data[i];
   }
 
   // Don't forget to release it
   env->ReleaseFloatArrayElements(*fArray, data, 0);
 
   return result;
}
```

##### 네이티브 함수를 통해 값을 반환합니다.

```cpp
JNIEXPORT
jint
JNICALL     
Java_com_autodesk_adn_jnitester_MainActivity_invokeMemberFuncFromNative
(
	JNIEnv *env,
	jobject callingObject,
	jobject obj
)
{
	jclass cls = env->GetObjectClass(obj);
	jmethodID methodId = env->GetMethodID(cls, "getFacetCount", "()I");
	int result = env->CallIntMethod(obj, methodId);
	    
	//Return the facet count (an int)  
	return result;
}
```

##### 네이티브 코드에서 자바 객체 인스턴스 만들기

```cpp
JNIEXPORT
jobject
JNICALL
Java_com_autodesk_adn_jnitester_MainActivity_createObjectFromNative
(
	JNIEnv *env,
	jobject callingObject,
	jint param
)
{
	jclass cls = env->FindClass("com/autodesk/adn/jnitester/MeshData");
	jmethodID methodId = env->GetMethodID(cls, "<init>", "(I)V");
	jobject obj = env->NewObject(cls, methodId, param);
 
	return obj;
}
```

#### 3. 자바와 네이티브 코드 간에 객체의 배열 전달하기

##### 객체 배열을 네이티브 코드로 전달하는 자바 메서드를 만듭니다.

List 또는 객체들을 네이티브 코드에 전달할 수 없습니다. 배열로 전달해야 합니다.

```java
public native int processObjectArrayFromNative(MeshData[] objArray);
```

##### 네이티브 코드에서 자바 객체 배열(jobjectArray) 읽기

```cpp
JNIEXPORT
jint
JNICALL
Java_com_autodesk_adn_jnitester_MainActivity_processObjectArrayFromNative
(
	JNIEnv *env,
	jobject callingObject,
	jobjectArray objArray
)
{
	int resultSum = 0;
	 
	int len = env->GetArrayLength(objArray);
	 
	//Get all the objects in the array
	for(int i=0; i<len; ++i)
	{
	    jobject obj = (jobject) env->GetObjectArrayElement(objArray, i);
	 
	    resultSum += getFacetCount(env, obj);
	}
	 
	return resultSum;
}
```

##### 네이티브 코드에서 객체 배열 반환하기

```cpp
JNIEXPORT 
jobjectArray 
JNICALL 
Java_ProcessInformation_getAllProcessPid
(
	JNIEnv*env,
	jobject obj
)
{
	//Create a vector (an array) of Strings and add items to it
   vector<string>vec;
	vec.push_back("Ranjan.B.M");
	vec.push_back("Mithun.V");
	vec.push_back("Preetham.S.N");
	vec.push_back("Karthik.S.G");
	cout<< vec[0];

	//Instantiate your object Array and return it!
	jclass clazz = (env)->FindClass("java/lang/String");
	jobjectArray objarray = (env)->NewObjectArray(vec.size() ,clazz ,0);
	for(int i=0; i < vec.size(); i++) {
		string s = vec[i];
		cout<< vec[i] <<endl;
		jstring js = (env)->NewStringUTF(s.c_str());
		(env)->SetObjectArrayElement(objarray, i, js);
	}
	return objarray;
}
```

## 참고자료
 * [Android NDK: Passing complex data between Java and JNI methods](http://adndevblog.typepad.com/cloud_and_mobile/2013/08/android-ndk-passing-complex-data-to-jni.html)
 * [Pass, return and convert to vectors list of lists over JNI](https://stackoverflow.com/questions/10813346/pass-return-and-convert-to-vectors-list-of-lists-over-jni)
 * [Manipulating Java Objects in Native Code](https://www.developer.com/java/data/manipulating-java-objects-in-native-code.html)
 * [JNI Tips](https://developer.android.com/training/articles/perf-jni.html)
 * [Add C and C++ Code to Your Project](https://developer.android.com/studio/projects/add-native-code.html)
 * [googlesamples/android-ndk](https://github.com/googlesamples/android-ndk)