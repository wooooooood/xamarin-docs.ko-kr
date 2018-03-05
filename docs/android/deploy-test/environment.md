---
title: "Xamarin.Android 환경"
ms.topic: article
ms.prod: xamarin
ms.assetid: 67BFD4E1-276C-4B9F-9BD8-A5218D2BD529
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 9ba8fc1a82e932c01b8a07b49d9ae11ad1ceb81c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinandroid-environment"></a>Xamarin.Android 환경

## <a name="execution-environment"></a>실행 환경

*실행 환경*은 프로그램 실행에 영향을 주는 환경 변수와 Android 시스템 속성의 집합입니다. Android 시스템 속성은 `adb shell setprop` 명령으로 설정할 수 있지만 환경 변수는 `debug.mono.env` 시스템 속성을 지정하여 설정할 수 있습니다.

```shell
## Enable GREF logging
adb shell setprop debug.mono.log gref

## Set the MONO_LOG_LEVEL and MONO_LOG_MASK environment variables
## so that additional Mono messages will be written to `adb logcat`.
adb shell setprop debug.mono.env "'MONO_LOG_LEVEL=info|MONO_LOG_MASK=asm'"
```

Android 시스템 속성은 대상 장치의 모든 프로세스에 대해 설정됩니다.

Xamarin.Android 4.6부터 시스템 속성 및 환경 변수는 모두 앱별로 프로젝트에 *환경 파일*을 추가하여 설정하거나 재정의할 수 있습니다. 환경 파일은 [`AndroidEnvironment`의 **빌드 동작**](~/android/deploy-test/building-apps/build-process.md)을 사용하는 Unix 형식의 일반 텍스트 파일입니다.
환경 파일에는 *key=value* 형식이 있는 줄이 포함됩니다.
주석은 `#`으로 시작하는 줄입니다. 빈 줄은 무시됩니다.

*key*가 대문자로 시작될 경우 *key*는 환경 변수로 취급되며, 프로세스 시작 시 지정된 *value*로 환경 변수를 설정하기 위해 **setenv**(3)가 사용됩니다.

*key*가 소문자로 시작하는 경우 *key*가 Android 시스템 속성으로 취급되고 *value*는 *default value*가 됩니다. Android 시스템 속성 서버에서 Xamarin.Android 실행 동작을 제어하는 Android 시스템 속성이 먼저 조회되고, 값이 없을 경우 환경 파일에 지정된 값이 사용됩니다. 이는 진단을 위해 환경 파일에서 제공되는 값을 재정의할 때 `adb shell setprop`를 사용하도록 허용하기 위한 것입니다.

## <a name="xamarinandroid-environment-variables"></a>Xamarin.Android 환경 변수

Xamarin.Android는 `adb shell setprop debug.mono.env` 또는 `$(AndroidEnvironment)` 빌드 동작을 통해 설정할 수 있는 `XA_HTTP_CLIENT_HANDLER_TYPE` 변수를 지원합니다.

<a name="XA_HTTP_CLIENT_HANDLER_TYPE" />

### `XA_HTTP_CLIENT_HANDLER_TYPE`

[HttpMessageHandler](https://msdn.microsoft.com/en-us/library/system.net.http.httpmessagehandler(v=vs.118).aspx)에서 상속되어야 하며 [`HttpClient()` 기본 생성자](https://msdn.microsoft.com/en-us/library/hh138077(v=vs.118).aspx)로 생성되는 어셈블리에 정규화된 형식.

Xamarin.Android 6.1에서는 이 환경 변수가 기본적으로 설정되지 않으며 [HttpClientHandler](https://msdn.microsoft.com/en-us/library/system.net.http.httpclienthandler(v=vs.118).aspx)가 사용됩니다.

또는 `Xamarin.Android.Net.AndroidClientHandler` 값을 지정하여 네트워크 액세스에 Android에서 지원하는 경우 TLS 1.2의 사용을 *허가*하는 [`java.net.URLConnection`](https://developer.xamarin.com/api/type/Java.Net.URLConnection/)을 사용할 수 있습니다.

Xamarin.Android 6.1에 추가되었습니다.

## <a name="xamarinandroid-system-properties"></a>Xamarin.Android 시스템 속성

Xamarin.Android는 `adb shell setprop` 또는 `$(AndroidEnvironment)` 빌드 동작을 통해 설정할 수 있는 다음과 같은 시스템 속성을 지원합니다.

* `debug.mono.debug`
* `debug.mono.env`
* `debug.mono.gc`
* `debug.mono.log`
* `debug.mono.max_grefc`
* `debug.mono.profile`
* `debug.mono.runtime_args`
* `debug.mono.trace`
* `debug.mono.wref`
* `XA_HTTP_CLIENT_HANDLER_TYPE`

### `debug.mono.debug`

`debug.mono.debug` 시스템 속성의 값은 정수입니다. `1`일 경우 프로세스가 `mono --debug`로 "시작된 것처럼" 동작합니다.
이는 일반적으로 디버거에서 앱을 시작하지 않아도 스택 추적 등에서 파일 및 줄 정보를 보여줍니다.

### `debug.mono.env`

`|`로 구분된 환경 변수 목록을 포함합니다.

### `debug.mono.gc`

`debug.mono.debug` 시스템 속성의 값은 정수입니다.
`1`일 경우 GC 정보가 로깅됩니다.

이는 `debug.mono.log` 시스템 속성이 `gc`를 포함하도록 하는 것과 동일합니다.

### `debug.mono.log`

Xamarin.Android가 `adb logcat`에 로깅할 추가 정보를 제어합니다.
쉼표로 구분된 문자열(`,`)이며, 다음 값 중 하나를 포함합니다.

* `all`: *모든* 메시지를 인쇄합니다. 이는 `lref` 메시지를 포함하므로 좋은 방법이 아닙니다.
* `assembly`: `.apk` 및 어셈블리 구문 분석 메시지를 인쇄합니다.
* `gc`: GC 관련 메시지를 인쇄합니다.
* `gref`: JNI 전역 참조 메시지를 인쇄합니다.
* `lref`: JNI 논리 참조 메시지를 인쇄합니다.  
    *참고*: 이는 *실제로* `adb logcat`을 스팸 처리합니다.  
    Xamarin.Android 5.1에서는 *gigantic*.Avoid를 받을 수 있는 `.__override__/lrefs.txt` 파일도  
    만듭니다.
* `timing`: 일부 메서드 타이밍 정보를 인쇄합니다. 이는 또한 `.__override__/methods.txt` 및 `.__override__/counters.txt` 파일도 만듭니다.


### `debug.mono.max_grefc`

`debug.mono.max_grefc` 시스템 속성의 값은 정수입니다.
이 값은 대상 장치에 대해 기본으로 검색된 최대 GREF 수를 *재정의*합니다.

*참고:* 이 값은 조만간 **environment.txt** 파일과 함께 사용할 수 없게 되므로 `adb shell setprop
debug.mono.max_grefc`와만 함께 사용할 수 있습니다.

### `debug.mono.profile`

`debug.mono.profile` 시스템 속성은 프로파일러를 활성화합니다.
`mono --profile` 옵션에 해당하며 이와 동일한 값을 사용합니다. (자세한 내용은 [**mono**(1)](http://docs.go-mono.com/?link=man%3amono(1)) 기본 페이지를 참조하세요.)

### `debug.mono.runtime_args`

`debug.mono.runtime_args` 시스템 속성에는 **mono**로 구문 분석해야 하는 추가 옵션이 있습니다.

### `debug.mono.trace`

`debug.mono.trace` 시스템 속성은 추적을 활성화합니다.
`mono --trace` 옵션에 해당하며 이와 동일한 값을 사용합니다. (자세한 내용은 [**mono**(1)](http://docs.go-mono.com/?link=man%3amono(1)) 기본 페이지를 참조하세요.)

일반적으로 *사용하지 않습니다*. 추적을 사용하면 `adb logcat` 출력이 스팸 처리되어 프로그램 동작 속도가 현저히 느려지고, 프로그램 동작이 변경됩니다(오류 조건이 추가될 수 있음).

하지만 *경우에 따라* 추가 조사를 수행할 수 있습니다.

### `debug.mono.wref`

`debug.mono.wref` 시스템 속성을 사용하면 기본으로 검색된 JNI 약한 참조 메커니즘을 재정의할 수 있습니다. 지원되는 값은 두 가지가 있습니다.

* `jni`: `JNIEnv::NewWeakGlobalRef()`에서 만들고 `JNIEnv::DeleteWeakGlobalREf()`에서 파괴하는 JNI 약한 참조를 사용합니다.
* `java`: `java.lang.WeakReference` 인스턴스를 참조하는 JNI 전역 참조를 사용합니다.

API-7 이하와 API-19(Kit Kat)에서는 ART가 활성화된 경우 기본적으로 `java`가 사용됩니다. (API-8은 `jni` 참조를 추가했고, ART는 `jni` 참조를 *중단*했습니다.)

이 시스템 속성은 테스트와 특정 형태의 조사에 유용합니다.
*일반적으로* 변경해서는 안 됩니다.

### <a name="xahttpclienthandlertype"></a>XA\_HTTP\_CLIENT\_HANDLER\_TYPE

Xamarin.Android 6.1에 처음 도입된 이 환경 변수는 `HttpClient`에서 사용할 기본 `HttpMessageHandler` 구현을 선언합니다. 기본적으로 이 변수는 설정되지 않으며, Xamarin.Android는 `HttpClientHandler`를 사용합니다.

```shell
XA_HTTP_CLIENT_HANDLER_TYPE=Xamarin.Android.Net.AndroidClientHandler
```

> [!NOTE]
> **참고:** 기본 Android 장치가 TLS 1.2를 지원해야 합니다.
Android 5.0 이상은 TLS 1.2를 지원합니다.


## <a name="example"></a>예

```shell
## Comments are lines which start with '#'
## Blank lines are ignored.


## Enable GREF messages to `adb logcat`
debug.mono.log=gref

## Clear out a Mono environment variable to decrease logging
MONO_LOG_LEVEL=
```



## <a name="related-links"></a>관련 링크

- [전송 계층 보안](~/cross-platform/app-fundamentals/transport-layer-security.md)
