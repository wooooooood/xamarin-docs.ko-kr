---
title: Xamarin.iOS 앱에서 iTunesMetadata.plist 파일
description: 이 문서에서는 테스트 또는 엔터프라이즈 배포를 위해 임시 배포를 사용하여 iOS 애플리케이션에 관한 정보를 iTunes에 제공하는 데 사용되는 iTunesMetadata.plist 파일에 대해 설명합니다.
ms.prod: xamarin
ms.assetid: 70676eba-6a99-4a3a-bccc-84359fe9c2c3
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 63a5ed357a903700ea89d858bcde9798ddf97942
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "76724622"
---
# <a name="the-itunesmetadataplist-file-in-xamarinios-apps"></a>Xamarin.iOS 앱에서 iTunesMetadata.plist 파일

_이 문서에서는 테스트 또는 엔터프라이즈 배포를 위해 임시 배포를 사용하여 iOS 애플리케이션에 관한 정보를 iTunes에 제공하는 데 사용되는 iTunesMetadata.plist 파일에 대해 설명합니다._

iOS 애플리케이션(iTunes 앱 스토어의 판매 또는 무료 릴리스용)이 iTunes Connect에 만들어지면, 개발자가 애플리케이션의 장르, 하위 장르, 저작권 표시, 지원되는 iOS 디바이스 및 필요한 디바이스 기능과 같은 정보를 지정할 수 있습니다. 임시 배포를 통해 테스터 또는 엔터프라이즈 사용자에게 배달되는 iOS 애플리케이션의 경우 이 정보가 누락됩니다.

누락된 정보를 임시 배포에 제공하기 위해 선택적인 `iTunesMetadata.plist` 파일을 만들어 애플리케이션 IPA 파일에 포함할 수 있습니다. 이 plist 파일은 지정된 iOS 애플리케이션에 대한 정보를 정의하는 키/값 쌍이 포함된 특수 형식의 XML 파일입니다(Apple의 [속성 목록 프로그래밍 가이드](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/PropertyLists/Introduction/Introduction.html) 참조).

<a name="iTunesMetadata_contents" />

## <a name="the-itunesmetadataplist-contents"></a>iTunesMetadata.plist 내용

다음은 임시 배포에 대한 iTunes 정보를 정의하는 데 사용되는 일반적인 `iTunesMetadata.plist` 파일의 예제입니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>UIRequiredDeviceCapabilities</key>
    <dict>
        <key>armv7</key>
        <true/>
        <key>front-facing-camera</key>
        <true/>
    </dict>
    <key>artistName</key>
    <string>Company, Inc.</string>
    <key>bundleDisplayName</key>
    <string>App Name</string>
    <key>bundleShortVersionString</key>
    <string>1.5.1</string>
    <key>bundleVersion</key>
    <string>1.5.1</string>
    <key>copyright</key>
    <string>© 2015 Company, Inc.</string>
    <key>drmVersionNumber</key>
    <integer>0</integer>
    <key>fileExtension</key>
    <string>.app</string>
    <key>gameCenterEnabled</key>
    <false/>
    <key>gameCenterEverEnabled</key>
    <false/>
    <key>genre</key>
    <string>Games</string>
    <key>genreId</key>
    <integer>6014</integer>
    <key>itemName</key>
    <string>App Name</string>
    <key>kind</key>
    <string>software</string>
    <key>playlistArtistName</key>
    <string>Company, Inc.</string>
    <key>playlistName</key>
    <string>App Name</string>
    <key>releaseDate</key>
    <string>2015-11-18T03:23:10Z</string>
    <key>s</key>
    <integer>143441</integer>
    <key>softwareIconNeedsShine</key>
    <false/>
    <key>softwareSupportedDeviceIds</key>
    <array>
        <integer>9</integer>
    </array>
    <key>softwareVersionBundleId</key>
    <string>com.company.appid</string>
    <key>subgenres</key>
    <array>
        <dict>
            <key>genre</key>
            <string>Puzzle</string>
            <key>genreId</key>
            <integer>7012</integer>
        </dict>
        <dict>
            <key>genre</key>
            <string>Word</string>
            <key>genreId</key>
            <integer>7019</integer>
        </dict>
    </array>
    <key>versionRestrictions</key>
    <integer>16843008</integer>
</dict>
</plist>

```

개별 키에 대한 값은 아래에서 자세히 설명합니다.

### <a name="uirequireddevicecapabilities"></a>UIRequiredDeviceCapabilities

`UIRequiredDeviceCapabilities` 키를 사용하면 iTunes에서 지정된 iOS 디바이스에 설치되기 전에 iOS 애플리케이션에 필요한 디바이스의 특정 기능을 인식할 수 있습니다. 이 키는 기능(`<key>...</key>`)의 사전(`<dict>...</dict>`) 및 각 기능에 대한 부울 값으로 제공됩니다. 기능의 값이 `true`이면 해당 기능이 장치에 있어야 합니다. `false`이면 해당 기능이 디바이스에 없어야 합니다. 예를 들어:

```xml
<key>UIRequiredDeviceCapabilities</key>
<dict>
    <key>armv7</key>
    <true/>
    <key>front-facing-camera</key>
    <true/>
</dict>
```

이 애플리케이션을 디바이스에 설치하려면 먼저 iOS 디바이스에서 ARM7 명령 집합을 지원하고 전면 카메라가 있어야 한다고 지정합니다. 허용되는 값의 전체 목록은 Apple의 [UIRequiredDeviceCapabilities](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html#//apple_ref/doc/uid/TP40009252-SW3) 설명서를 참조하세요.

### <a name="artistname-and-playlistartistname"></a>artistName 및 playlistArtistName

`artistName` 및 `playlistArtistName` 키를 사용하여 iTunes에 표시할 iOS 애플리케이션을 만든 회사의 이름을 정의합니다. 예:

```xml
<key>artistName</key>
<string>Company, Inc.</string>
...
<key>playlistArtistName</key>
<string>Company, Inc.</string>
```

### <a name="bundledisplayname-itemname-and-playlistname"></a>bundleDisplayName, itemName 및 playlistName

`bundleDisplayName`, `itemName` 및 `playlistName` 키를 사용하여 iTunes 내부에 표시할 iOS 애플리케이션의 이름을 정의합니다. 예:

```xml
<key>bundleDisplayName</key>
<string>App Name</string>
...
<key>itemName</key>
<string>App Name</string>
...
<key>playlistName</key>
<string>App Name</string>
```

### <a name="bundleshortversionstring-and-bundleversion"></a>bundleShortVersionString 및 bundleVersion

`bundleShortVersionString` 및 `bundleVersion` 키를 사용하여 iTunes에 표시할 iOS 애플리케이션의 버전 번호를 정의합니다. 예:

```xml
<key>bundleShortVersionString</key>
<string>1.5.1</string>
<key>bundleVersion</key>
<string>1.5.1</string>
```

### <a name="softwareversionbundleid"></a>softwareVersionBundleId

`softwareVersionBundleId` 키를 사용하여 iOS 애플리케이션에 대한 번들 ID를 지정합니다. 예:

```xml
<key>softwareVersionBundleId</key>
<string>com.company.appid</string>
```

### <a name="copyright"></a>저작권

`copyright` 키를 사용하여 iTunes에 표시되는 저작권 표시를 정의합니다. 예:

```xml
<key>copyright</key>
<string>© 2015 Company, Inc.</string>
```

### <a name="releasedate"></a>releaseDate

`releaseDate` 키를 사용하여 iTunes에 표시할 iOS 애플리케이션의 릴리스 날짜를 제공합니다. 예:

```xml
<key>releaseDate</key>
<string>2015-11-18T03:23:10Z</string>
```

### <a name="softwareiconneedsshine"></a>softwareIconNeedsShine

`softwareIconNeedsShine` 키를 사용하여 iOS 6(및 이전 버전)에 대해 _반짝이는 강조 표시_가 iOS 애플리케이션의 아이콘에 필요한지 여부를 iTunes에 알립니다. 예:

```xml
<key>softwareIconNeedsShine</key>
<false/>
```

### <a name="gamecenterenabled-and-gamecentereverenabled"></a>gameCenterEnabled 및 gameCenterEverEnabled

`gameCenterEnabled` 및 `gameCenterEverEnabled` 키를 사용하여 이 iOS 애플리케이션이 Apple의 Game Center를 지원하는지 여부를 iTunes에 알립니다. 예:

```xml
<key>gameCenterEnabled</key>
<false/>
<key>gameCenterEverEnabled</key>
<false/>
```

### <a name="genre-genreid-and-subgenres"></a>genre, genreId 및 subgenres

`genre` 및 `genreId` 키를 사용하여 iOS 애플리케이션이 속한 장르를 iTunes에 알립니다. 예:

```xml
<key>genre</key>
<string>Games</string>
<key>genreId</key>
<integer>6014</integer>
```

필요에 따라 `subgenres` 키를 사용하여 iOS 애플리케이션에 대해 최대 두 개의 하위 장르를 추가로 정의할 수 있습니다. 예:

```xml
<key>subgenres</key>
<array>
    <dict>
        <key>genre</key>
        <string>Puzzle</string>
        <key>genreId</key>
        <integer>7012</integer>
    </dict>
    <dict>
        <key>genre</key>
        <string>Word</string>
        <key>genreId</key>
        <integer>7019</integer>
    </dict>
</array>
```

iOS 애플리케이션의 경우 현재 Apple에서 정의한 장르 및 장르 ID는 다음과 같습니다.

[!include[](~/ios/includes/table-appstore.md)]

### <a name="softwaresupporteddeviceids"></a>softwareSupportedDeviceIds

`softwareSupportedDeviceIds` 키를 사용하여 이 iOS 애플리케이션이 지원하는 iOS 디바이스를 iTunes에 알립니다. 예:

```xml
<key>softwareSupportedDeviceIds</key>
<array>
    <integer>9</integer>
</array>
```

다음 값을 사용할 수 있는 위치:

- 1 – 클래식 iPhone
- 2 – iPod Touch
- 4 – iPad
- 9 – 최신 iPhone

### <a name="standard-keys"></a>표준 키

다음 키는 iOS 애플리케이션의 모든 `iTunesMetadata.plist` 파일에 포함되어 있으며 항상 동일한 값을 갖습니다.

```xml
<key>drmVersionNumber</key>
<integer>0</integer>
<key>fileExtension</key>
<string>.app</string>
...
<key>kind</key>
<string>software</string>
...
<key>s</key>
<integer>143441</integer>
...
<key>versionRestrictions</key>
<integer>16843008</integer>
```

<a name="iTunesMetadata_creating" />

## <a name="creating-an-itunesmetadataplist-file"></a>iTunesMetadata.plist 파일 만들기

# <a name="visual-studio-for-mac"></a>[Mac용 Visual Studio](#tab/macos)

 Mac용 Visual Studio에서 `iTunesMetadata.plist` 파일을 사용하는 경우 두 가지 옵션이 있습니다.

- Mac용 Visual Studio의 시각적 plist 편집기를 사용하여 파일을 만들고 유지 관리합니다.
- 일반 텍스트 편집기에서 파일을 만들고 유지 관리합니다.

 두 가지 옵션에 대해서는 아래에서 자세히 설명합니다.

### <a name="using-the-visual-plist-editor"></a>시각적 Plist 편집기 사용

다음을 수행합니다.

1. **솔루션 탐색기**에서 Xamarin.iOS 프로젝트 파일을 마우스 오른쪽 단추로 클릭하고, **추가** > **새 파일...** 을 차례로 선택합니다.
2. [새 파일] 대화 상자에서 **iOS** > **속성 목록**을 차례로 선택합니다.

    ![](itunesmetadata-images/image01.png "Select iOS Property List")
3. **이름**에 대해 `iTunesMetadata`를 입력하고 **새로 만들기** 단추를 클릭합니다.
4. 편집하기 위해 **솔루션 탐색기**에서 `iTunesMetadata.plist` 파일을 두 번 클릭하여 엽니다.

    ![](itunesmetadata-images/image02.png "The iTunesMetadata.plist editor")
5. 녹색 **+** 를 클릭하여 새 항목을 만들고, 키 이름으로 `UIRequiredDeviceCapabilities`를 입력합니다.

    ![](itunesmetadata-images/image03.png "Create a new entry and enter UIRequiredDeviceCapabilities as the key name")
6. **문자열** 값 형식을 클릭하고, 팝업 목록에서 **사전**을 선택합니다.

    ![](itunesmetadata-images/image04.png "Select Dictionary from the popup list")
7. 속성 이름의 왼쪽에서 접혀 있는 부분을 클릭하여 사전 항목을 표시합니다.

    ![](itunesmetadata-images/image05.png "Reveal the dictionary entries")
8. **새 항목 추가** 텍스트를 클릭한 다음, 녹색 **+** 를 클릭하여 사전에 항목을 추가합니다.

    ![](itunesmetadata-images/image06.png "Add an entry to the dictionary")
9. 키 이름으로 `armv7`을 입력하고, **부울** 형식을 선택하고, 값으로 **예**를 입력합니다.

    ![](itunesmetadata-images/image07.png "Enter armv7 for the key name, select a type of Boolean and enter Yes as the value")
10. 필요한 모든 키/값 쌍으로 `iTunesMetadata.plist` 파일을 채울 때까지 위의 단계를 반복합니다(자세한 내용은 위의 [iTunesMetadata.plist 내용](#iTunesMetadata_contents) 섹션 참조).

11. 변경 내용을 plist 파일에 저장합니다.

### <a name="using-a-plain-text-editor"></a>일반 텍스트 편집기 사용

다음을 수행합니다.

1. 일반 텍스트 편집기에서 새 텍스트 파일을 만들고, 이름을 `iTunesMetadata.plist`로 지정합니다.
2. 위의 [iTunesMetadata.plist 내용](#iTunesMetadata_contents) 섹션에 있는 내용 예제를 복사합니다.
3. 내용을 파일에 붙여넣고 필요에 따라 편집합니다.
4. 파일을 저장하고 Mac용 Visual Studio로 돌아갑니다.
5. **솔루션 탐색기**에서 Xamarin.iOS 프로젝트 파일을 마우스 오른쪽 단추로 클릭하고, **추가** > **기존 파일...** 을 차례로 선택합니다.
6. [파일 열]기 대화 상자에서 위에서 만든 `iTunesMetadata.plist` 파일을 선택하고 **확인** 단추를 클릭합니다.
7. 이 파일에서 **없음**으로 설정된 **빌드 작업**은 그대로 둡니다.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Visual Studio용 Xamarin 플러그 인은 `Info.plist` 및 `Entitlement.plist` 파일에 대한 시각적 편집기만 지원하므로 표준 텍스트 편집기에서 `iTunesMetadata.plist` 파일을 만들고 Xamarin.iOS 프로젝트에 수동으로 포함해야 합니다.

다음을 수행합니다.

1. 일반 텍스트 편집기에서 새 텍스트 파일을 만들고, 이름을 `iTunesMetadata.plist`로 지정합니다.
2. 위의 [iTunesMetadata.plist 내용](#iTunesMetadata_contents) 섹션에 있는 내용 예제를 복사합니다.
3. 내용을 파일에 붙여넣고 필요에 따라 편집합니다.
4. 파일을 저장하고 Visual Studio로 돌아갑니다.
5. **솔루션 탐색기**에서 Xamarin.iOS 프로젝트 파일을 마우스 오른쪽 단추로 클릭하고, **추가** > **기존 파일...** 을 차례로 선택합니다.
6. [파일 열기] 대화 상자에서 위에서 만든 `iTunesMetadata.plist` 파일을 선택하고 **열기** 단추를 클릭합니다.
7. 이 파일에서 **없음**으로 설정된 **빌드 작업**은 그대로 둡니다.

-----

나중에 IDE에서 IPA를 빌드할 준비가 되면 이 `iTunesMetadata.plist` 파일을 선택합니다.

## <a name="summary"></a>요약

이 문서에서는 iTunes에 임시로 배달된 iOS 애플리케이션을 알리는 데 사용할 수 있는 `iTunesMetadata.plist` 파일에 대해 설명했습니다. plist 파일의 표준 키와 Visual Studio 및 Mac용 Visual Studio에서 파일을 만들고 유지 관리하는 방법에 대해 설명했습니다.

## <a name="related-links"></a>관련 링크

- [App Store 배포](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [iTunes Connect에서 앱 구성](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [앱 스토어에 게시](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [사내 배포](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [임시 배포](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [IPA 지원](~/ios/deploy-test/app-distribution/ipa-support.md)
- [문제 해결](~/ios/deploy-test/troubleshooting.md)
