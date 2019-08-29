---
title: 글꼴
ms.prod: xamarin
ms.assetid: 3F543FC5-FDED-47F8-8D2C-481FCC98BFDA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/09/2018
ms.openlocfilehash: 1d0341af35d3c580141c5bfc76e9f170cd7ff4c5
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70119099"
---
# <a name="fonts"></a>글꼴

## <a name="overview"></a>개요

API 수준 26부터 Android SDK를 사용 하면 레이아웃이 나 drawables과 마찬가지로 글꼴이 리소스로 처리 될 수 있습니다. [Android 지원 라이브러리 26 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/26.1.0.1) 은 api 수준 14 이상을 대상으로 하는 앱에 새 font API를 이식할 수 있습니다.

API 26을 대상으로 하거나 Android 지원 라이브러리 v26를 설치한 후에는 두 가지 방법으로 Android 응용 프로그램에서 글꼴을 사용할 수 있습니다.

1. **Android 리소스로 글꼴 패키지** &ndash; 이렇게 하면 응용 프로그램에서 글꼴을 항상 사용할 수 있지만 apk의 크기가 증가 합니다.
2. **글꼴 다운로드** Android는 글꼴 공급자의 글꼴 다운로드도 지원 합니다. &ndash; 글꼴 공급자는 글꼴이 장치에 이미 있는지 확인 합니다. 필요한 경우 글꼴이 다운로드 되어 장치에 캐시 됩니다. 이 글꼴은 여러 응용 프로그램 간에 공유할 수 있습니다.

비슷한 글꼴이 나 여러 스타일을 포함할 수 있는 글꼴은 _글꼴 패밀리_로 그룹화 될 수 있습니다. 이를 통해 개발자는 가중치와 같은 글꼴의 특정 특성을 지정할 수 있습니다. 그러면 Android에서 글꼴 패밀리의 적절 한 글꼴을 자동으로 선택 합니다.

Android Support Library v26는 글꼴에 대 한 포트 지원을 API 수준 26으로 백 합니다. 이전 API 수준을 대상으로 지정 하는 경우 `app` `android:` 네임 스페이스 및 `app:` 네임 스페이스를 사용 하 여 XML 네임 스페이스를 선언 하 고 다양 한 글꼴 특성의 이름을 지정 해야 합니다. `android:` 네임 스페이스를 사용 하는 경우에는 API 레벨 25를 실행 하는 장치에서 글꼴이 표시 되지 않습니다. 예를 들어 다음 XML 코드 조각은 API 수준 14 이상에서 작동 하는 새 [_글꼴 패밀리_](#font_families) 리소스를 선언 합니다.

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

글꼴이 Android 응용 프로그램에 적절 한 방식으로 제공 되는 한, [ `fontFamily` 특성](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily)을 설정 하 여 UI 위젯에 적용할 수 있습니다. 예를 들어 다음 코드 조각은 TextView에서 글꼴을 표시 하는 방법을 보여 줍니다.

```xml
<TextView
    android:text="The quick brown fox jumped over the lazy dog."
    android:fontFamily="@font/sourcesanspro_regular"
    app:fontFamily="@font/sourcesanspro_regular"
    android:textAppearance="?android:attr/textAppearanceLarge"
    android:layout_width="match_parent"
    android:layout_height="wrap_content" />
```

이 가이드에서는 먼저 글꼴을 Android 리소스로 사용한 다음 런타임에 글꼴을 다운로드 하는 방법에 대해 설명 하는 방법에 대해 설명 합니다.

## <a name="fonts-as-a-resource"></a>리소스로 서의 글꼴

Android APK에 글꼴을 패키지 하면 응용 프로그램에서 항상 사용할 수 있습니다. 글꼴 파일 ( .TTF 또는입니다. Xamarin.ios 프로젝트의 **Resources** 폴더에 있는 하위 디렉터리에 파일을 복사 하 여 다른 리소스와 마찬가지로 xamarin.ios 응용 프로그램에 추가 됩니다. Fonts 리소스는 프로젝트의 **resources** 폴더에 있는 **글꼴** 하위 디렉터리에 보관 됩니다.

> [!NOTE]
> 글꼴은 **Androidresource** 의 **빌드 작업** 을 수행 해야 합니다. 그렇지 않으면 최종 apk에 패키지 되지 않습니다. IDE에서 빌드 작업을 자동으로 설정 해야 합니다.

비슷한 글꼴 파일이 여러 개 있는 경우 (예: 다른 가중치가 나 스타일을 사용 하는 동일한 글꼴) 글꼴 패밀리로 그룹화 할 수 있습니다.

<a name="font_families" />

### <a name="font-families"></a>글꼴 패밀리

글꼴 패밀리는 가중치와 스타일이 다른 글꼴 집합입니다. 예를 들어, 굵게 또는 기울임꼴 글꼴을 위한 별도의 글꼴 파일이 있을 수 있습니다. 글꼴 패밀리는 `font` **리소스/글꼴** 디렉터리에 보관 된 XML 파일의 요소에 의해 정의 됩니다. 각 글꼴 패밀리에는 고유한 XML 파일이 있어야 합니다.

글꼴 패밀리를 만들려면 먼저 **리소스/글꼴** 폴더에 모든 글꼴을 추가 합니다. 그런 다음 글꼴 패밀리에 대 한 글꼴 폴더에 새 XML 파일을 만듭니다. XML 파일의 이름에는 참조 되는 글꼴에 대 한 선호도 또는 관계가 없습니다. 리소스 파일은 모든 올바른 Android 리소스 파일 이름일 수 있습니다. 이 XML 파일에는 하나 `font-family` `font` 이상의 요소를 포함 하는 루트 요소가 있습니다. 각 `font` 요소는 글꼴의 특성을 선언 합니다.

다음 XML은 다양 한 글꼴 가중치를 정의 하는 _원본 San Pro_ 글꼴의 글꼴 패밀리에 대 한 예입니다. 이 파일은 **sourcesanspro .xml**이라는 **리소스/글꼴** 폴더에 파일로 저장 됩니다.

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

특성 `fontStyle` 에는 두 가지 가능한 값이 있습니다.

- **보통** &ndash; 일반 글꼴
- **기울임꼴** &ndash; 기울임꼴 글꼴

특성 `fontWeight` 은 CSS `font-weight` 특성에 해당 하며 글꼴 두께를 참조 합니다. 100-900 범위의 값입니다. 다음 목록에서는 일반적인 글꼴 가중치 값과 해당 이름을 설명 합니다.

- **씬** &ndash; 100
- **Extra Light** &ndash; 200
- **Light** &ndash; 300
- **보통** &ndash; 400
- **보통** &ndash; 500
- **반 굵게** &ndash; 600
- **굵게** &ndash; 700
- **매우 굵게** &ndash; 800
- **검정** &ndash; 900

글꼴 패밀리를 정의한 후에는 레이아웃 파일에서 `fontFamily`, `textStyle`및 `fontWeight` 특성을 설정 하 여 선언적으로 사용할 수 있습니다.  예를 들어 다음 XML 코드 조각은 600 가중치 글꼴 (보통) 및 기울임꼴 텍스트 스타일을 설정 합니다.

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

### <a name="programmatically-assigning-fonts"></a>프로그래밍 방식으로 글꼴 할당

메서드를 [`Resources.GetFont`](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int)) 사용 하 여 개체를 [`Typeface`](https://developer.android.com/reference/android/graphics/Typeface.html) 검색 하는 방식으로 글꼴을 프로그래밍 방식으로 설정할 수 있습니다. 많은 보기 `TypeFace` 에 글꼴을 위젯에 할당 하는 데 사용할 수 있는 속성이 있습니다. 이 코드 조각은 TextView에서 프로그래밍 방식으로 글꼴을 설정 하는 방법을 보여 줍니다.

```csharp
Android.Graphics.Typeface typeface = this.Resources.GetFont(Resource.Font.caveat_regular);
textView1.Typeface = typeface;
textView1.Text = "Changed the font";
```

이 `GetFont` 메서드는 글꼴 패밀리 내에서 첫 번째 글꼴을 자동으로 로드 합니다.  특정 스타일과 일치 하는 글꼴을 로드 하려면 메서드를 `Typeface.Create` 사용 합니다. 이 메서드는 지정 된 스타일과 일치 하는 글꼴을 로드 하려고 합니다. 예를 들어이 코드 조각은 **리소스/글꼴**에 정의 된 `Typeface` 글꼴 패밀리에서 굵은 개체를 로드 하려고 합니다.

```csharp
var typeface = Typeface.Create("<FONT FAMILY NAME>", Android.Graphics.TypefaceStyle.Bold);
textView1.Typeface = typeface;
```

## <a name="downloading-fonts"></a>글꼴 다운로드

Android는 응용 프로그램 리소스로 글꼴을 패키징하는 대신 원격 원본에서 글꼴을 다운로드할 수 있습니다. 이를 통해 APK의 크기를 줄이는 것이 좋습니다.

_글꼴은 글꼴 공급자_의 지원과 함께 다운로드 됩니다. 이는 장치의 모든 응용 프로그램에 대 한 글꼴 다운로드 및 캐싱을 관리 하는 특수 한 콘텐츠 공급자입니다. Android 8.0에는 [Google 글꼴 리포지토리에서](http://fonts.google.com)글꼴을 다운로드 하는 글꼴 공급자가 포함 되어 있습니다. 이 기본 글꼴 공급자는 Android 지원 라이브러리 v26를 사용 하 여 API 수준 14로 백 포팅 됩니다.

앱에서 글꼴을 요청 하면 글꼴 공급자는 먼저 글꼴이 장치에 이미 있는지 확인 합니다. 그렇지 않으면 글꼴 다운로드를 시도 합니다. 글꼴을 다운로드할 수 없는 경우 Android에서 기본 시스템 글꼴을 사용 합니다. 글꼴이 다운로드 되 면 초기 요청을 만든 앱 뿐만 아니라 장치의 모든 응용 프로그램에서 사용할 수 있습니다.

글꼴을 다운로드 하도록 요청 하는 경우 앱은 글꼴 공급자를 직접 쿼리하지 않습니다. 대신, 앱은 [`FontsContract`](https://developer.android.com/reference/android/provider/FontsContract.html) API 인스턴스 ( [`FontsContractCompat`](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html) 또는 지원 라이브러리 26이 사용 되는 경우)를 사용 합니다.  

Android 8.0는 다음과 같은 두 가지 방법으로 글꼴 다운로드를 지원 합니다.

1. **다운로드 가능한 글꼴을 리소스로 선언** &ndash; 앱은 XML 리소스 파일을 통해 Android에 다운로드 가능한 글꼴을 선언할 수 있습니다. 이러한 파일에는 앱이 시작 될 때 Android에서 비동기적으로 글꼴을 다운로드 하 고 장치에서 캐시 하는 데 필요한 모든 메타 데이터가 포함 됩니다.
2. **프로그래밍 방식으로** &ndash; Android api level 26의 api를 사용 하면 응용 프로그램이 실행 되는 동안 응용 프로그램에서 프로그래밍 방식으로 글꼴을 다운로드할 수 있습니다. 앱은 지정 된 `FontRequest` 글꼴에 대 한 개체를 만들고이 개체를 `FontsContract` 클래스에 전달 합니다. 는 `FontsContract` 를`FontRequest` 가져와서 _글꼴 공급자_에서 글꼴을 검색 합니다. Android에서 동기적으로 글꼴을 다운로드 합니다. 을 `FontRequest` 만드는 예제는이 가이드의 뒷부분에 나와 있습니다.

사용 되는 방법에 관계 없이, 글꼴을 다운로드 하기 전에 Xamarin.ios 응용 프로그램에 추가 해야 하는 리소스 파일을 확인할 수 있습니다. 먼저 글꼴을 글꼴 패밀리의 일부로 **리소스/글꼴** 디렉터리의 XML 파일에 선언 해야 합니다. 이 코드 조각은 Android 8.0 (또는 Support Library v26)와 함께 제공 되는 기본 글꼴 공급자를 사용 하 여 [Google Fonts Open Source collection](https://fonts.google.com) 에서 글꼴을 다운로드 하는 방법의 예입니다.

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

요소 `font-family` 는 다음 특성을 포함 하며, Android에서 글꼴을 다운로드 하는 데 필요한 정보를 선언 합니다.

1. 가 나 **Providerauthority** &ndash; 요청에 사용할 글꼴 공급자의 인증 기관입니다.
2. **글꼴 패키지** &ndash; 요청에 사용할 글꼴 공급자의 패키지입니다. 공급자의 id를 확인 하는 데 사용 됩니다.
3. **글꼴 쿼리** &ndash; 글꼴 공급자가 요청 된 글꼴을 찾는 데 도움이 되는 문자열입니다. 글꼴 쿼리 정보는 글꼴 공급자에 따라 다릅니다. [`QueryBuilder`](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs) [다운로드 가능한 글꼴](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/) 샘플 앱의 클래스는 Google fonts Open Source Collection의 글꼴에 대 한 쿼리 형식에 대 한 일부 정보를 제공 합니다.
4. 가 나 **Provider인증서** &ndash; 공급자에 서명 해야 하는 인증서의 해시 집합 목록이 포함 된 리소스 배열입니다.

글꼴이 정의 되 면 다운로드와 관련 된 _글꼴 인증서_ 에 대 한 정보를 제공 해야 할 수 있습니다.

### <a name="font-certificates"></a>글꼴 인증서

장치에 글꼴 공급자가 미리 설치 되어 있지 않거나 앱이 `Xamarin.Android.Support.Compat` 라이브러리를 사용 하는 경우 Android를 사용 하려면 글꼴 공급자의 보안 인증서가 필요 합니다. 이러한 인증서는 **리소스/값** 디렉터리에 유지 되는 배열 리소스 파일에 나열 됩니다.

예를 들어 다음 XML은 **Resources/values/fonts_cert** 라는 이름으로 지정 되며 Google 글꼴 공급자에 대 한 인증서를 저장 합니다.

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

이러한 리소스 파일이 준비 되 면 앱에서 글꼴을 다운로드할 수 있습니다.

### <a name="declaring-downloadable-fonts-as-resources"></a>다운로드 가능한 글꼴을 리소스로 선언

Android는 **Androidmanifest**에서 다운로드 가능한 글꼴을 나열 하 여 앱이 처음 시작 될 때 해당 글꼴을 비동기식으로 다운로드 합니다. 글꼴 자체는 다음과 유사 하 게 배열 리소스 파일에 나열 됩니다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <array name="downloadable_fonts" translatable="false">
        <item>@font/vt323</item>
    </array>
</resources>
```

이러한 글꼴을 다운로드 하려면 `application` 요소를 자식으로 추가 `meta-data` 하 여 **androidmanifest** 에 선언 해야 합니다. 예를 들어 다운로드 가능한 글꼴이 **Resources/values/downloadable_fonts**의 리소스 파일에 선언 된 경우이 코드 조각을 매니페스트에 추가 해야 합니다.

```xml
<meta-data android:name="downloadable_fonts" android:resource="@array/downloadable_fonts" />
```

### <a name="downloading-a-font-with-the-font-apis"></a>글꼴 Api를 사용 하 여 글꼴 다운로드

개체를 [`FontRequest`](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html) 인스턴스화하고 `FontContractCompat.RequestFont` 메서드에 전달 하 여 프로그래밍 방식으로 글꼴을 다운로드할 수 있습니다. 메서드 `FontContractCompat.RequestFont` 는 먼저 장치가 장치에 있는지 확인 한 다음 필요한 경우에는 해당 글꼴이 비동기적으로 쿼리 되 고 앱의 글꼴을 다운로드 합니다. 에서 `FontRequest` 글꼴을 다운로드할 수 없는 경우 Android에서 기본 시스템 글꼴을 사용 합니다.

개체 `FontRequest` 는 글꼴 공급자가 글꼴을 찾아서 다운로드 하는 데 사용 되는 정보를 포함 합니다. 에 `FontRequest` 는 다음 네 가지 정보가 필요 합니다.

1. **글꼴 공급자 인증 기관** &ndash; 요청에 사용할 글꼴 공급자의 인증 기관입니다.
2. **글꼴 패키지** &ndash; 요청에 사용할 글꼴 공급자의 패키지입니다. 공급자의 id를 확인 하는 데 사용 됩니다.
3. **글꼴 쿼리** &ndash; 글꼴 공급자가 요청 된 글꼴을 찾는 데 도움이 되는 문자열입니다. 글꼴 쿼리 정보는 글꼴 공급자에 따라 다릅니다. 문자열의 세부 정보는 글꼴 공급자와 관련이 있습니다. [`QueryBuilder`](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/DownloadableFonts/QueryBuilder.cs) [다운로드 가능한 글꼴](https://github.com/xamarin/monodroid-samples/blob/master/android-o/DownloadableFonts/) 샘플 앱의 클래스는 Google fonts Open Source Collection의 글꼴에 대 한 쿼리 형식에 대 한 일부 정보를 제공 합니다.
4. **글꼴 공급자 인증서** &ndash; 공급자에 서명 해야 하는 인증서의 해시 집합 목록을 포함 하는 리소스 배열입니다.

이 코드 조각은 새 `FontRequest` 개체를 인스턴스화하는 예제입니다.

```csharp
FontRequest request = new FontRequest("com.google.android.gms.fonts", "com.google.android.gms", <FontToDownload>, Resource.Array.com_google_android_gms_fonts_certs);
```

이전 코드 조각은 `FontToDownload` Google Fonts Open Source collection의 글꼴을 돕는 쿼리입니다.

를 `FontContractCompat.RequestFont` 메서드에 전달 `FontRequest` 하기 전에 다음 두 개체를 만들어야 합니다.

- **`FontsContractCompat.FontRequestCallback`** &ndash; 확장 해야 하는 추상 클래스입니다. 가 완료 되 면 `RequestFont` 호출 되는 콜백입니다. Xamarin Android 앱은 `FontsContractCompat.FontRequestCallback` `OnTypefaceRequestFailed` 및 `OnTypefaceRetrieved`를 하위 클래스 하 고 재정의 해야 하며, 각각 다운로드에 실패 하거나 성공할 경우 수행할 작업을 제공 해야 합니다.
- **`Handler`** 이는 필요한 `Handler` 경우에서 `RequestFont` 스레드에 글꼴을 다운로드 하는 데 사용 하는입니다. &ndash; UI 스레드에서 글꼴을 다운로드 해서는 안 됩니다.

이 코드 조각은 Google Fonts Open Source C# collection에서 비동기적으로 글꼴을 다운로드 하는 클래스의 예입니다. `FontRequestCallback` 인터페이스를 구현 하 고이 완료 되 C# 면 `FontRequest` 이벤트를 발생 시킵니다.

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

이 도우미를 사용 하려면 새 `FontDownloadHelper` 가 만들어지고 이벤트 처리기가 할당 됩니다.  

```csharp
var fontHelper = new FontDownloadHelper();

fontHelper.FontDownloaded += (object sender, FontDownloadEventArg e) =>
{
    //React to the request
};
fontHelper.DownloadFonts(this); // this is an Android Context instance.
```

## <a name="summary"></a>요약

이 가이드에서는 다운로드 가능한 글꼴 및 글꼴을 리소스로 지원 하기 위해 Android 8.0의 새 Api에 대해 설명 했습니다. 기존 글꼴을 APK에 포함 하 고 레이아웃에서 사용 하는 방법에 대해 설명 했습니다. 또한 Android 8.0는 프로그래밍 방식으로 또는 리소스 파일에서 글꼴 메타 데이터를 선언 하 여 글꼴 공급자의 글꼴 다운로드를 지 원하는 방법도 설명 합니다.

## <a name="related-links"></a>관련 링크

- [fontFamily](https://developer.android.com/reference/android/widget/TextView.html#attr_android:fontFamily)
- [FontConfig](https://developer.android.com/reference/android/text/FontConfig.html)
- [FontRequest](https://developer.android.com/reference/android/support/v4/provider/FontRequest.html)
- [FontsContractCompat](https://developer.android.com/reference/android/support/v4/provider/FontsContractCompat.html)
- [Resources.GetFont](https://developer.android.com/reference/android/content/res/Resources.html#getFont(int))
- [Typeface](https://developer.android.com/reference/android/graphics/Typeface.html)
- [Android 지원 라이브러리 26 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.Compat/)
- [Android에서 글꼴 사용](https://www.youtube.com/watch?v=TfB-TsLFJdM)
- [CSS 글꼴 두께 사양](https://www.w3.org/TR/css-fonts-3/#font-weight-numeric-values)
- [Google Fonts 오픈 소스 컬렉션](https://fonts.google.com/)
- [원본 San Pro](https://fonts.google.com/specimen/Source+Sans+Pro)
