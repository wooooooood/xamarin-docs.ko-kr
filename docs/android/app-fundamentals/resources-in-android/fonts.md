---
title: 글꼴
ms.prod: xamarin
ms.assetid: 3F543FC5-FDED-47F8-8D2C-481FCC98BFDA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/09/2018
ms.openlocfilehash: c7953748e79bd43bc14601c1f0ea05d1a36adf08
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668130"
---
# <a name="fonts"></a>글꼴

## <a name="overview"></a>개요

글꼴을 레이아웃 마찬가지로 리소스로 간주 사용 하면 Android SDK API 레벨 26 부터는 또는 드로어 블 합니다. 합니다 [Android 지원 라이브러리 26 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/26.1.0.1) 새 글꼴 API의 대상 API 수준 14 이상 해당 앱을 백 포팅 됩니다.

대상으로 하는 API 26 또는 Android 지원 라이브러리 v26 설치 후 Android 응용 프로그램에서 글꼴을 사용 하는 방법은 두 가지 있습니다.

1. **패키지는 Android 리소스와 글꼴** &ndash; 하면 글꼴은 항상 응용 프로그램에 사용할 수 있지만 APK의 크기를 증가 합니다.
2. **글꼴 다운로드** &ndash; Android에서 글꼴 다운로드도 지원를 _글꼴 공급자_합니다. 글꼴 공급자는 이미 글꼴 장치의 적용 인지를 확인 합니다. 필요한 경우 글꼴을 다운로드 하 고 장치에 캐시 됩니다. 이 글꼴은 여러 응용 프로그램 간에 공유할 수 있습니다.

비슷한 글꼴 (또는 몇 가지 다른 스타일을 가질 수 있는 글꼴)으로 그룹화 할 수 있습니다 _글꼴 패밀리_합니다. 개발자가 글꼴의 가중치 등의 특정 특성을 지정할 수 있으며 Android가 글꼴 패밀리에서 적절 한 글꼴을 자동으로 선택 합니다.

Android 지원 라이브러리 v26 API 수준 26 글꼴에 대 한 백 포팅 지원이 됩니다. 선언 해야 하는 경우 이전 API 수준을 대상으로 하는 `app` XML 네임 스페이스를 사용 하 여 다양 한 글꼴 특성의 이름을 합니다 `android:` 네임 스페이스 및 `app:` 네임 스페이스입니다. 경우는 `android:` 네임 스페이스를 사용 하 고, 다음 글꼴 25 또는 API 레벨을 실행 하는 표시 된 장치 수 없습니다. 예를 들어이 XML 조각을 선언 새 [ _글꼴 패밀리_ ](#font_families) API 수준 14 이상에서 작동 하는 리소스:

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

     <font  android:font="@font/sourcesanspro_regular"
            android:fontStyle="normal"
            android:fontWeight="400"
            app:font="@font/sourcesanspro_regular"
            app:fontStyle="normal"
            app:fontWeight="400" />

</font-family>
```

글꼴은 Android 응용 프로그램에 적절 한 방식으로 제공 됩니다,으로 이러한에 적용할 수 UI 위젯 설정 하 여 합니다 [ `fontFamily` 특성](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily)합니다. 예를 들어, 다음 코드 조각은 글꼴 TextView를 표시 하는 방법을 보여 줍니다.

```xml
<TextView
    android:text="The quick brown fox jumped over the lazy dog."
    android:fontFamily="@font/sourcesanspro_regular"
    app:fontFamily="@font/sourcesanspro_regular"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

이 가이드는 먼저 Android 리소스로 글꼴을 사용 하는 방법에 논의 하 고 런타임에 글꼴을 다운로드 하는 방법에 논의 하 이동 합니다.

## <a name="fonts-as-a-resource"></a>리소스로 글꼴

Android APK로 글꼴 패키징 응용 프로그램에 사용할 수 있는 항상 인지 확인 합니다. 글꼴 파일 (중 하나를 합니다. TTF 또는 합니다. OTF 파일)에서 하위 디렉터리에 파일을 복사 하 여 다른 리소스와 마찬가지로 Xamarin.Android 응용 프로그램에 추가 됩니다는 **리소스** Xamarin.Android 프로젝트의 폴더입니다. 글꼴 리소스에 유지 되는 **글꼴** 의 하위 디렉터리를 **리소스** 프로젝트의 폴더입니다.

> [!NOTE]
> 글꼴 있어야를 **빌드 작업** 의 **AndroidResource** 또는 최종 APK에 패키지 되지 것입니다. IDE에서 빌드 작업을 자동으로 설정 되어야 합니다.

유사한 여러 글꼴 파일 (예를 들어, 서로 다른 가중치 또는 스타일을 사용 하 여 동일한 글꼴) 경우 글꼴 패밀리로 그룹화 하는 것이 같습니다.

<a name="font_families" />

### <a name="font-families"></a>글꼴 패밀리

글꼴 패밀리는 다른 가중치가 및 스타일에 있는 글꼴의 집합입니다. 예를 들어, 굵게 또는 기울임꼴 글꼴에 대 한 별도 글꼴 파일 있을 수 있습니다. 글꼴 패밀리 정의한 `font` 에 보관 되는 XML 파일에 요소를 **리소스/글꼴** 디렉터리입니다. 각 글꼴 패밀리에 대 한 자체 XML 파일이 있어야 합니다.

글꼴 패밀리를 만들려면 먼저 모든 글꼴을 추가 합니다 **글꼴리소스/** 폴더입니다. 그런 다음 글꼴 패밀리에 대 한 글꼴 폴더에 새 XML 파일을 만듭니다. XML 파일의 이름에 없거나 선호도 관계; 참조 되는 글꼴 리소스 파일에 모든 올바른 Android 리소스 파일 이름일 수 있습니다. 이 XML 파일을 루트 갖습니다 `font-family` 하나 이상 포함 하는 요소 `font` 요소입니다. 각 `font` 요소 글꼴의 특성을 선언 합니다.

다음 XML은 예제에 대 한 글꼴 패밀리입니다 합니다 _소스 San Pro_ 많은 다양 한 글꼴 두께 정의 하는 글꼴입니다. 이에 파일로 저장 되는 **리소스/글꼴** 라는 폴더 **sourcesanspro.xml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family xmlns:android="http://schemas.android.com/apk/res/android"
             xmlns:app="http://schemas.android.com/apk/res-auto">
    <font android:font="@font/sourcesanspro_regular"
          android:fontStyle="normal"
          android:fontWeight="400"
          app:font="@font/sourcesanspro_regular"
          app:fontStyle="normal"
          app:fontWeight="400" />
    <font android:font="@font/sourcesanspro_bold"
          android:fontStyle="normal"
          android:fontWeight="800"
          app:font="@font/sourcesanspro_bold"
          app:fontStyle="normal"
          app:fontWeight="800" />
    <font android:font="@font/sourcesanspro_italic"
          android:fontStyle="italic"
          android:fontWeight="400"
          app:font="@font/sourcesanspro_italic"
          app:fontStyle="italic"
          app:fontWeight="400" />
</font-family>
```

`fontStyle` 특성에 두 개의 가능한 값:

* **정상적인** &ndash; 보통 글꼴
* **기울임꼴** &ndash; 여 기울임꼴 글꼴

합니다 `fontWeight` CSS에 해당 하는 특성 `font-weight` 특성 및 글꼴의 두께 가리킵니다. 100-900 범위의 값입니다. 다음 목록에는 일반 글꼴 두께 값 및 해당 이름을 설명합니다.

* **씬** &ndash; 100
* **Extra Light** &ndash; 200
* **Light** &ndash; 300
* **Normal** &ndash; 400
* **중간** &ndash; 500
* **굵게 semi** &ndash; 600
* **굵은** &ndash; 700
* **아주 굵게** &ndash; 800
* **Black** &ndash; 900

설정 하 여 선언적으로 사용할 수 있습니다 글꼴 패밀리를 정의한 후 합니다 `fontFamily`, `textStyle`, 및 `fontWeight` 레이아웃 파일의 특성입니다.  예를 들어 다음 XML 조각 (일반) 600 가중치 글꼴 및 기울임꼴 텍스트 스타일을 설정합니다.

```xml
<TextView
    android:text="Sans Source Pro semi-bold italic, 600 weight, italic"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:fontFamily="@font/sourcesanspro"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:gravity="center_horizontal"
    android:fontWeight="600"
    android:textStyle="italic"
    />
```

### <a name="programmatically-assigning-fonts"></a>글꼴을 프로그래밍 방식으로 할당

글꼴을 사용 하 여 프로그래밍 방식으로 설정할 수 있습니다 합니다 [ `Resources.GetFont` ](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int)) 검색 하는 메서드를 [ `Typeface` ](https://developer.android.com/reference/android/graphics/Typeface.html) 개체입니다. 많은 보기에는 `TypeFace` 위젯에 글꼴을 할당할 수 있는 속성입니다. 이 코드 조각은 글꼴 TextView를 프로그래밍 방식으로 설정 하는 방법을 보여 줍니다.

```csharp
Android.Graphics.Typeface typeface = this.Resources.GetFont(Resource.Font.caveat_regular);
textView1.Typeface = typeface;
textView1.Text = "Changed the font";
```

`GetFont` 메서드 글꼴 패밀리 내 첫 번째 글꼴을 자동으로 로드 됩니다.  특정 스타일과 일치 하는 글꼴을 로드 하려면 사용 된 `Typeface.Create` 메서드. 이 메서드는 지정된 된 스타일과 일치 하는 글꼴을 로드 하려고 합니다. 예를 들어이 코드 조각은 굵게 표시 된 로드를 시도 `Typeface` 개체에 정의 된 글꼴 패밀리 **리소스/글꼴**:

```csharp
var typeface = Typeface.Create("<FONT FAMILY NAME>", Android.Graphics.TypefaceStyle.Bold);
textView1.Typeface = typeface;
```

## <a name="downloading-fonts"></a>글꼴 다운로드

응용 프로그램 리소스로 글꼴 패키징, 대신 Android 원격 원본에서 글꼴을 다운로드할 수 있습니다. 이 APK의 크기를 줄이는 바람직하지 적용을 해야 합니다.

글꼴의 도움으로 다운로드 됩니다는 _글꼴 공급자_합니다. 다운로드 및 장치에서 모든 응용 프로그램에 글꼴의 캐싱을 관리 하는 특수 한 콘텐츠 공급자입니다. Android 8.0에서 글꼴을 다운로드 하려면 글꼴 공급자를 포함 합니다 [Google 글꼴 리포지토리](http://fonts.google.com)합니다. 이 기본 글꼴 공급자는 Android 지원 라이브러리 v26 사용 하 여 API 수준 14로 백 포팅 합니다.

앱의 글꼴에 대 한 요청 하면 글꼴 공급자는 글꼴이 이미 장치에 있는지 먼저 확인 됩니다. 그렇지 않은 경우 해당 글꼴을 다운로드 하려고 합니다. 글꼴 다운로드 한 다음 Android 수 없을 경우 기본 시스템 글꼴을 사용 합니다. 글꼴 다운로드 되 면 초기 요청을 수행 하는 앱 뿐 아니라 장치의 모든 응용 프로그램에 제공 됩니다.

글꼴 다운로드 요청을 만들면 앱을 직접으로 글꼴 공급자를 쿼리하지 않습니다. 앱은의 인스턴스를 사용 하는 대신 합니다 [ `FontsContract` ](https://developer.android.com/reference/android/provider/FontsContract.html) API (또는 [ `FontsContractCompat` ](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html) 지원 라이브러리 26를 사용 하는 경우).  

Android 8.0에서는 두 가지 방법으로 다운로드 글꼴을 지원합니다.

1. **다운로드할 수 있는 글꼴을 리소스로 선언** &ndash; 앱 XML 리소스 파일을 통해 Android에 대 한 다운로드 가능한 글꼴을 선언할 수 있습니다. 이러한 파일에는 Android 앱을 시작 하 고 장치에서 캐시 하는 글꼴을 비동기적으로 다운로드 해야 하는 메타 데이터를 모두 포함 됩니다.
2. **프로그래밍 방식으로** &ndash; Android API 레벨 26 Api 응용 프로그램이 실행 되는 동안 글꼴을 프로그래밍 방식으로 다운로드 하는 응용 프로그램을 허용 합니다. 앱을 만듭니다는 `FontRequest` 지정된 된 글꼴에 대 한 개체 및이 개체는 `FontsContract` 클래스입니다. `FontsContract` 사용 합니다 `FontRequest` 에서 글꼴을 검색 하 고는 _글꼴 공급자_합니다. Android는 글꼴을 동기적으로 다운로드 합니다. 만드는 예제는 `FontRequest` 이 가이드의 뒷부분에 표시 됩니다.

어떤 방법을 사용 하는 것에 관계 없이 글꼴 전에 Xamarin.Android 응용 프로그램에 추가 되어야 하는 리소스 파일을 다운로드할 수 있습니다. XML 파일에 글꼴 먼저 선언 해야 합니다 **리소스/글꼴** 글꼴 패밀리 일부로 디렉터리입니다. 이 코드 조각은 글꼴에서 다운로드 하는 방법의 예는 [Google 글꼴 오픈 소스 컬렉션](https://fonts.google.com) Android 8.0 (또는 지원 라이브러리 v26) 함께 제공 되는 기본 글꼴 공급자를 사용 하 여:

```xml
<?xml version="1.0" encoding="utf-8"?>
<font-family xmlns:android="http://schemas.android.com/apk/res/android"
             xmlns:app="http://schemas.android.com/apk/res-auto"
             android:fontProviderAuthority="com.google.android.gms.fonts"
             android:fontProviderPackage="com.google.android.gms"
             android:fontProviderQuery="VT323"
             android:fontProviderCerts="@array/com_google_android_gms_fonts_certs"
             app:fontProviderAuthority="com.google.android.gms.fonts"
             app:fontProviderPackage="com.google.android.gms"
             app:fontProviderQuery="VT323"
             app:fontProviderCerts="@array/com_google_android_gms_fonts_certs"
>
</font-family>
```

`font-family` 요소 Android 필요한 글꼴을 다운로드 하는 정보를 선언에 다음 특성을 포함 합니다.

1. **fontProviderAuthority** &ndash; 요청에 사용할 글꼴 공급자의 기관입니다.
2. **fontPackage** &ndash; 요청에 사용할 글꼴 공급자에 대 한 패키지 있습니다. 이 공급자의 id를 확인 하려면 사용 됩니다.
3. **fontQuery** &ndash; 글꼴 공급자 요청 된 글꼴을 찾는 데 도움이 되는 문자열입니다. 글꼴 쿼리에 대 한 세부 정보는 글꼴 공급자와 관련이 있습니다. 합니다 [ `QueryBuilder` ](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs) 클래스를 [다운로드 글꼴](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/) 샘플 앱 일부 정보를 제공 하는 쿼리 형식에 글꼴에 대 한 Google 글꼴 오픈 소스 컬렉션에서 합니다.
4. **fontProviderCerts** &ndash; 공급자를 사용 하 여 서명 해야 하는 인증서에 대 한 해시 집합의 목록으로는 리소스 배열입니다.

글꼴을 정의 하는 방법에 대 한 정보를 제공 해야 할 수 있습니다 합니다 _글꼴 인증서_ 다운로드에 포함 합니다.

### <a name="font-certificates"></a>글꼴 인증서

글꼴 공급자 장치에 사전 설치 되지 않은 경우 또는 앱을 사용 하는 경우는 `Xamarin.Android.Support.Compat` Android 라이브러리 글꼴 공급자의 보안 인증서를 요구 합니다. 이러한 인증서에 보관 되는 배열 리소스 파일에 나열 됩니다 **리소스/값** 디렉터리입니다.

예를 들어, 다음 XML 이름이 **Resources/values/fonts_cert.xml** Google 글꼴 공급자에 대 한 인증서를 저장 합니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="com_google_android_gms_fonts_certs">
        <item>@array/com_google_android_gms_fonts_certs_dev</item>
        <item>@array/com_google_android_gms_fonts_certs_prod</item>
    </array>
    <string-array name="com_google_android_gms_fonts_certs_dev">
        <item>
            MIIEqDCCA5CgAwIBAgIJANWFuGx90071MA0GCSqGSIb3DQEBBAUAMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbTAeFw0wODA0MTUyMzM2NTZaFw0zNTA5MDEyMzM2NTZaMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbTCCASAwDQYJKoZIhvcNAQEBBQADggENADCCAQgCggEBANbOLggKv+IxTdGNs8/TGFy0PTP6DHThvbbR24kT9ixcOd9W+EaBPWW+wPPKQmsHxajtWjmQwWfna8mZuSeJS48LIgAZlKkpFeVyxW0qMBujb8X8ETrWy550NaFtI6t9+u7hZeTfHwqNvacKhp1RbE6dBRGWynwMVX8XW8N1+UjFaq6GCJukT4qmpN2afb8sCjUigq0GuMwYXrFVee74bQgLHWGJwPmvmLHC69EH6kWr22ijx4OKXlSIx2xT1AsSHee70w5iDBiK4aph27yH3TxkXy9V89TDdexAcKk/cVHYNnDBapcavl7y0RiQ4biu8ymM8Ga/nmzhRKya6G0cGw8CAQOjgfwwgfkwHQYDVR0OBBYEFI0cxb6VTEM8YYY6FbBMvAPyT+CyMIHJBgNVHSMEgcEwgb6AFI0cxb6VTEM8YYY6FbBMvAPyT+CyoYGapIGXMIGUMQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEQMA4GA1UEChMHQW5kcm9pZDEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDEiMCAGCSqGSIb3DQEJARYTYW5kcm9pZEBhbmRyb2lkLmNvbYIJANWFuGx90071MAwGA1UdEwQFMAMBAf8wDQYJKoZIhvcNAQEEBQADggEBABnTDPEF+3iSP0wNfdIjIz1AlnrPzgAIHVvXxunW7SBrDhEglQZBbKJEk5kT0mtKoOD1JMrSu1xuTKEBahWRbqHsXclaXjoBADb0kkjVEJu/Lh5hgYZnOjvlba8Ld7HCKePCVePoTJBdI4fvugnL8TsgK05aIskyY0hKI9L8KfqfGTl1lzOv2KoWD0KWwtAWPoGChZxmQ+nBli+gwYMzM1vAkP+aayLe0a1EQimlOalO762r0GXO0ks+UeXde2Z4e+8S/pf7pITEI/tP+MxJTALw9QUWEv9lKTk+jkbqxbsh8nfBUapfKqYn0eidpwq2AzVp3juYl7//fKnaPhJD9gs=
        </item>
    </string-array>
    <string-array name="com_google_android_gms_fonts_certs_prod">
        <item>
            MIIEQzCCAyugAwIBAgIJAMLgh0ZkSjCNMA0GCSqGSIb3DQEBBAUAMHQxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1Nb3VudGFpbiBWaWV3MRQwEgYDVQQKEwtHb29nbGUgSW5jLjEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDAeFw0wODA4MjEyMzEzMzRaFw0zNjAxMDcyMzEzMzRaMHQxCzAJBgNVBAYTAlVTMRMwEQYDVQQIEwpDYWxpZm9ybmlhMRYwFAYDVQQHEw1Nb3VudGFpbiBWaWV3MRQwEgYDVQQKEwtHb29nbGUgSW5jLjEQMA4GA1UECxMHQW5kcm9pZDEQMA4GA1UEAxMHQW5kcm9pZDCCASAwDQYJKoZIhvcNAQEBBQADggENADCCAQgCggEBAKtWLgDYO6IIrgqWbxJOKdoR8qtW0I9Y4sypEwPpt1TTcvZApxsdyxMJZ2JORland2qSGT2y5b+3JKkedxiLDmpHpDsz2WCbdxgxRczfey5YZnTJ4VZbH0xqWVW/8lGmPav5xVwnIiJS6HXk+BVKZF+JcWjAsb/GEuq/eFdpuzSqeYTcfi6idkyugwfYwXFU1+5fZKUaRKYCwkkFQVfcAs1fXA5V+++FGfvjJ/CxURaSxaBvGdGDhfXE28LWuT9ozCl5xw4Yq5OGazvV24mZVSoOO0yZ31j7kYvtwYK6NeADwbSxDdJEqO4k//0zOHKrUiGYXtqw/A0LFFtqoZKFjnkCAQOjgdkwgdYwHQYDVR0OBBYEFMd9jMIhF1Ylmn/Tgt9r45jk14alMIGmBgNVHSMEgZ4wgZuAFMd9jMIhF1Ylmn/Tgt9r45jk14aloXikdjB0MQswCQYDVQQGEwJVUzETMBEGA1UECBMKQ2FsaWZvcm5pYTEWMBQGA1UEBxMNTW91bnRhaW4gVmlldzEUMBIGA1UEChMLR29vZ2xlIEluYy4xEDAOBgNVBAsTB0FuZHJvaWQxEDAOBgNVBAMTB0FuZHJvaWSCCQDC4IdGZEowjTAMBgNVHRMEBTADAQH/MA0GCSqGSIb3DQEBBAUAA4IBAQBt0lLO74UwLDYKqs6Tm8/yzKkEu116FmH4rkaymUIE0P9KaMftGlMexFlaYjzmB2OxZyl6euNXEsQH8gjwyxCUKRJNexBiGcCEyj6z+a1fuHHvkiaai+KL8W1EyNmgjmyy8AW7P+LLlkR+ho5zEHatRbM/YAnqGcFh5iZBqpknHf1SKMXFh4dd239FJ1jWYfbMDMy3NS5CTMQ2XFI1MvcyUTdZPErjQfTbQe3aDQsQcafEQPD+nqActifKZ0Np0IS9L9kR/wbNvyz6ENwPiTrjV2KRkEjH78ZMcUQXg0L3BYHJ3lc69Vs5Ddf9uUGGMYldX3WfMBEmh/9iFBDAaTCK
        </item>
    </string-array>
</resources>
```

현재 위치에서 이러한 리소스 파일을 사용 하 여 앱이 글꼴을 다운로드할 수 있는 합니다.

### <a name="declaring-downloadable-fonts-as-resources"></a>다운로드할 수 있는 글꼴을 리소스로 선언

다운로드할 수 있는 글꼴을 나열 하 여 합니다 **AndroidManifest.XML**, 앱을 처음 시작할 때 Android 글꼴을 비동기적으로 다운로드 됩니다. 글꼴의 자체는이 이와 유사한 배열 리소스 파일에 나열 됩니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="downloadable_fonts" translatable="false">
        <item>@font/vt323</item>
    </array>
</resources>
```

이러한 글꼴을 다운로드 하려면에 선언할 필요가 **AndroidManifest.XML** 더하여 `meta-data` 의 자식으로는 `application` 요소. 예를 들어, 다운로드 가능한 글꼴에 있는 리소스 파일에 선언 된 경우 **Resources/values/downloadable_fonts.xml**를 매니페스트에 추가 되었으면이 코드 조각:

```xml
<meta-data android:name="downloadable_fonts" android:resource="@array/downloadable_fonts" />
```

### <a name="downloading-a-font-with-the-font-apis"></a>글꼴 Api 사용 하 여 글꼴 다운로드

프로그래밍 방식으로 인스턴스화하여 글꼴을 다운로드할 수 있기를 [ `FontRequest` ](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html) 개체에 전달 하는 `FontContractCompat.RequestFont` 메서드. `FontContractCompat.RequestFont` 메서드를 먼저 글꼴 장치에 존재 하는지 확인 하 고 다음 필요한 경우는 비동기적으로 쿼리 글꼴 공급자 및 앱에 대 한 글꼴을 다운로드 하려고 합니다. 경우 `FontRequest` Android는 기본 시스템 글꼴을 사용 하 여 글꼴을 다운로드할 수 없는 합니다.

`FontRequest` 개체를 찾아 글꼴 다운로드 글꼴 공급자에서 사용할 정보를 포함 합니다. `FontRequest` 네 가지 정보가 필요 합니다.

1. **글꼴 공급자 기관** &ndash; 요청에 사용할 글꼴 공급자의 기관입니다.
2. **글꼴 패키지** &ndash; 요청에 사용할 글꼴 공급자에 대 한 패키지 있습니다. 이 공급자의 id를 확인 하려면 사용 됩니다.
3. **글꼴 쿼리** &ndash; 글꼴 공급자 요청 된 글꼴을 찾는 데 도움이 되는 문자열입니다. 글꼴 쿼리에 대 한 세부 정보는 글꼴 공급자와 관련이 있습니다. 문자열의 세부 사항은 글꼴 공급자별으로 다릅니다. 합니다 [ `QueryBuilder` ](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs) 클래스를 [다운로드 글꼴](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/) 샘플 앱 일부 정보를 제공 하는 쿼리 형식에 글꼴에 대 한 Google 글꼴 오픈 소스 컬렉션에서 합니다.
4. **글꼴 공급자 인증서** &ndash; 공급자를 사용 하 여 서명 인증서에 대 한 해시 집합의 목록으로는 리소스 배열입니다.

이 코드 조각은 새 인스턴스화의 한 예로 `FontRequest` 개체:

```csharp
FontRequest request = new FontRequest("com.google.android.gms.fonts", "com.google.android.gms", <FontToDownload>, Resource.Array.com_google_android_gms_fonts_certs);
```

이전 코드 조각에서 `FontToDownload` Google 글꼴 오픈 소스 컬렉션에서 글꼴을 안내 하는 쿼리입니다.

전달 하기 전에 합니다 `FontRequest` 에 `FontContractCompat.RequestFont` 메서드를 작성 해야 하는 개체 두 개가:

* **`FontsContractCompat.FontRequestCallback`** &ndash; 이것이 확장 해야 하는 추상 클래스입니다. 콜백이 될 것이 될 때 호출 `RequestFont` 가 완료 합니다. Xamarin.Android 앱을 서브 클래스 해야 `FontsContractCompat.FontRequestCallback` 재정의 `OnTypefaceRequestFailed` 및 `OnTypefaceRetrieved`, 다운로드가 실패 하거나 각각 성공 하는 경우 수행할 작업을 제공 합니다.
* **`Handler`** &ndash; 이 `Handler` 에서 사용 됩니다 `RequestFont` 필요한 경우 스레드에서 글꼴을 다운로드 합니다. 글꼴 해야 **되지** UI 스레드에서 다운로드할 수 있습니다.

이 코드 조각은의 한 예로 C# Google 글꼴 오픈 소스 컬렉션에서 글꼴을 비동기적으로 다운로드 하는 클래스입니다. 구현 하는 `FontRequestCallback` 인터페이스를 발생 시킵니다를 C# 이벤트 때 `FontRequest` 완료 합니다.

```csharp
public class FontDownloadHelper : FontsContractCompat.FontRequestCallback
{
    // A very simple font query; replace as necessary
    public static readonly String FontToDownload = "Courgette";

    Android.OS.Handler Handler = null;

    public event EventHandler<FontDownloadEventArg> FontDownloaded = delegate
    {
        // just an empty delegate to avoid null reference exceptions.  
    };


    public void DownloadFonts(Context context)
    {
        FontRequest request = new FontRequest("com.google.android.gms.fonts", "com.google.android.gms",FontToDownload , Resource.Array.com_google_android_gms_fonts_certs);
        FontsContractCompat.RequestFont(context, request, this, GetHandlerThreadHandler());
    }

    public override void OnTypefaceRequestFailed(int reason)
    {
        base.OnTypefaceRequestFailed(reason);
        FontDownloaded(this, new FontDownloadEventArg(null));
    }

    public override void OnTypefaceRetrieved(Android.Graphics.Typeface typeface)
    {
        base.OnTypefaceRetrieved(typeface);
        FontDownloaded(this, new FontDownloadEventArg(typeface));
    }

    Handler GetHandlerThreadHandler()
    {
        if (Handler == null)
        {
            HandlerThread handlerThread = new HandlerThread("fonts");
            handlerThread.Start();
            Handler = new Handler(handlerThread.Looper);
        }
        return Handler;
    }
}

public class FontDownloadEventArg : EventArgs
{
    public FontDownloadEventArg(Android.Graphics.Typeface typeface)
    {
        Typeface = typeface;
    }
    public Android.Graphics.Typeface Typeface { get; private set; }
    public bool RequestFailed
    {
        get
        {
            return Typeface != null;
        }
    }
}
```

이 도우미를 사용 하려면 새 `FontDownloadHelper` 만들어지면 이벤트 처리기가 할당 됩니다.  

```csharp
var fontHelper = new FontDownloadHelper();

fontHelper.FontDownloaded += (object sender, FontDownloadEventArg e) =>
{
    //React to the request
};
fontHelper.DownloadFonts(this); // this is an Android Context instance.
```

## <a name="summary"></a>요약

이 가이드는 다운로드 가능한 글꼴 및 글꼴 리소스를 지원 하도록 Android 8.0의 새로운 Api를 설명 합니다. 해당 APK의 기존 글꼴을 포함 하 고 레이아웃을 사용 하는 방법을 설명 합니다. 또한 프로그래밍 방식으로 또는 리소스 파일의 글꼴 메타 데이터를 선언 하 여 Android 8.0 글꼴 공급자 로부터 다운로드 글꼴을 지원 하는 방법을 설명 합니다.

## <a name="related-links"></a>관련 링크

- [fontFamily](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily)
- [FontConfig](https://developer.android.com/reference/android/text/FontConfig.html)
- [FontRequest](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html)
- [FontsContractCompat](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html)
- [Resources.GetFont](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int))
- [Typeface](https://developer.android.com/reference/android/graphics/Typeface.html)
- [Android 지원 라이브러리 26 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/)
- [Android에서 글꼴을 사용 하 여](https://www.youtube.com/watch?v=TfB-TsLFJdM)
- [CSS 글꼴 두께 사양](https://www.w3.org/TR/css-fonts-3/#font-weight-numeric-values)
- [Google 글꼴 오픈 소스 컬렉션](https://fonts.google.com/)
- [Pro San 원본](https://fonts.google.com/specimen/Source+Sans+Pro)
