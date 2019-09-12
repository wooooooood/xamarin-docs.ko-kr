---
title: Xamarin.iOS에서 IPA 지원
description: 이 문서에서는 테스트 또는 내부 애플리케이션의 사내 배포를 위해 임시 배포를 사용하여 애플리케이션을 배포하는 데 사용할 수 있는 IPA 파일을 만드는 방법에 대해 설명합니다.
ms.prod: xamarin
ms.assetid: D253C2DB-852E-6FC6-C9FD-574730B8DB19
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/19/2017
ms.openlocfilehash: b9982f9102166aa6892be0819615f329a65fffbb
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70756437"
---
# <a name="ipa-support-in-xamarinios"></a>Xamarin.iOS에서 IPA 지원

_이 문서에서는 테스트 또는 내부 애플리케이션의 사내 배포를 위해 임시 배포를 사용하여 애플리케이션을 배포하는 데 사용할 수 있는 IPA 파일을 만드는 방법에 대해 설명합니다._

iTunes 앱 스토어를 통해 판매할 애플리케이션을 릴리스하는 것 외에도 다음 용도로 배포할 수 있습니다.

- **임시 테스트** - 알파 및 베타 테스트 목적으로 iOS 애플리케이션을 최대 100명의 사용자(특정 iOS 디바이스 UUID로 식별)에게 배포할 수 있습니다. Apple iOS 디바이스를 Apple 개발자 계정에 추가하는 방법에 대한 자세한 내용은 [개발용 iOS 디바이스 프로비전](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning) 설명서를 참조하고, 이 방식으로 배포하는 방법에 대한 자세한 내용은 [임시](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md) 가이드를 참조하세요.
- **사내/엔터프라이즈 배포** - iOS 애플리케이션은 Apple의 Developer Enterprise 프로그램 구성원 자격이 필요한 회사 내에서 내부적으로 배포할 수 있습니다. 사내 배포에 대한 자세한 내용은 [사내 배포](~/ios/deploy-test/app-distribution/in-house-distribution.md) 가이드에서 자세히 설명하고 있습니다.

두 경우 모두에서 올바른 배포 프로비전 프로필을 사용하여 IPA 패키지(특수 유형의 zip 파일)를 만들고 디지털 서명해야 합니다. 이 문서에서는 Mac 또는 Windows PC에서 iTunes를 사용하여 IPA 패키지를 빌드하고 iOS 디바이스에 패키지를 설치하는 데 필요한 단계에 대해 설명합니다.

<a name="iTunesMetadata" />

## <a name="the-itunesmetadataplist-file"></a>iTunesMetadata.plist 파일

iOS 애플리케이션(iTunes 앱 스토어의 판매 또는 무료 릴리스용)이 iTunes Connect에 만들어지면, 개발자가 애플리케이션의 장르, 하위 장르, 저작권 표시, 지원되는 iOS 디바이스 및 필요한 디바이스 기능과 같은 정보를 지정할 수 있습니다.

임시 또는 사내 배포를 통해 제공되는 iOS 애플리케이션에는 iTunes 및 사용자 디바이스에 표시될 수 있도록 이 정보를 지원하는 몇 가지 방법이 필요합니다. 기본적으로 프로젝트를 빌드할 때마다 작은 iTunesMetadata.plist 파일이 만들어지며 프로젝트 디렉터리에 저장됩니다.

또한 추가 정보를 배포에 제공하기 위해 사용자 지정 **iTunesMetadata.plist**를 만들 수도 있습니다. 이 파일의 내용 및 만드는 방법에 대한 자세한 내용은 [iTunesMetadata.plist 내용](~/ios/deploy-test/app-distribution/itunesmetadata.md#iTunesMetadata_contents) 및 [iTunesMetadata.plist 파일 만들기](~/ios/deploy-test/app-distribution/itunesmetadata.md#iTunesMetadata_creating) 설명서를 참조하세요.

<a name="iTunesArtwork" />

## <a name="itunes-artwork"></a>iTunes 아트워크

앱 스토어 이외의 수단을 통해 앱을 배달하는 경우 iTunes에서 애플리케이션을 나타내는 데 사용할 512x512 및 1024x1024 이미지도 포함해야 합니다.

iTunes 아트워크를 지정하려면 다음을 수행합니다.

1. 편집하기 위해 **솔루션 탐색기**에서 **Info.plist** 파일을 두 번 클릭하여 엽니다.
2. 편집기의 **iTunes 아트워크** 섹션으로 스크롤합니다.
3. 누락된 이미지가 있는 경우 편집기에서 썸네일을 클릭하고, **파일 열기** 대화 상자에서 원하는 iTunes 아트워크에 대한 이미지 파일을 선택하고, **확인** 또는 **열기** 단추를 누릅니다.
4. 애플리케이션에 필요한 이미지를 모두 지정할 때까지 이 단계를 반복합니다.

자세한 내용은 [iTunes 아트워크](~/ios/app-fundamentals/images-icons/app-icons.md) 설명서를 참조하세요.

<a name="createipa" />

## <a name="creating-an-ipa"></a>IPA 만들기

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

IPA 만들기는 이제 새 게시 워크플로에 내장됩니다. 이렇게 하려면 아래 지침에 따라 앱을 보관하고, 서명한 다음, IPA를 저장합니다.

플랫폼 간 솔루션용 IPA를 만들기 시작하기 전에 iOS 프로젝트를 시작 프로젝트로 선택했는지 확인합니다.

![](ipa-support-images/setasstartup.png "iOS 프로젝트를 시작 프로젝트로 선택")

### <a name="build-your-archive"></a>보관 빌드

IPA를 빌드하려면 애플리케이션의 릴리스 빌드에 대한 _보관_을 만들어야 합니다. 이 보관에는 앱 및 이를 식별하는 정보가 포함됩니다.

1. Mac용 Visual Studio에서 **릴리스 | 디바이스** 구성을 선택합니다.

    ![](ipa-support-images/buildxs01new.png "릴리스 | 디바이스 구성 선택")

1. **빌드** 메뉴에서 **게시를 위해 보관**을 선택합니다.

    ![](ipa-support-images/buildxs02new.png "게시를 위해 보관 선택")

1. 보관이 만들어지면 **보관** 보기가 표시됩니다.

    ![](ipa-support-images/buildxs03new.png "표시된 보관 보기")

### <a name="sign-and-distribute-your-app"></a>앱 서명 및 배포

보관용 애플리케이션을 빌드할 때마다 **보관 보기**가 자동으로 열리고, 보관된 모든 프로젝트가 솔루션별로 그룹화되어 표시됩니다. 이 보기에는 기본적으로 현재 열려 있는 솔루션만 표시됩니다. 보관이 있는 솔루션을 모두 보려면 **모든 보관 표시** 옵션을 클릭합니다.

나중에 생성된 모든 디버그 정보를 기호로 나타낼 수 있도록 고객에게 배포된 보관 파일(임시 또는 사내 배포)을 유지하는 것이 좋습니다.

앱 스토어가 아닌 경우 **iTunesMetadata.plist** 파일을 빌드하고, iTunes 아트워크 집합이 보관에 있는 경우 IPA에 자동으로 포함됩니다.

앱에 서명하고 배포할 준비를 하려면 다음을 수행합니다.

1. 아래 그림과 같이 **서명 및 배포...** 단추를 선택합니다.

    ![](ipa-support-images/buildxs04new.png "서명 및 배포... 선택")

1. 그러면 게시 마법사가 열립니다. **임시** 또는 **엔터프라이즈**(사내) 배포 채널을 선택하여 패키지를 만듭니다.

    ![](ipa-support-images/distribute01.png "임시 또는 엔터프라이즈 사내 배포 선택")

1. [프로비전 프로필] 화면에서 서명 ID 및 해당 프로비전 프로필을 선택하거나 다른 ID로 다시 서명합니다.

    ![](ipa-support-images/distribute02.png "서명 ID 및 해당 프로비전 프로필 선택")

1. 패키지의 세부 정보를 확인하고 **게시**를 클릭합니다.

    ![](ipa-support-images/distribute03.png "패키지 세부 정보 확인")

1. 마지막으로 IPA를 컴퓨터에 저장합니다.

    ![](ipa-support-images/distribute04.png "컴퓨터에 IPA 저장")

### <a name="building-via-the-command-line-on-mac"></a>명령줄을 통한 빌드(Mac에서)

CI 환경과 같은 경우에는 명령줄을 통해 IPA를 빌드해야 할 수도 있습니다. 이렇게 하려면 아래 단계를 수행합니다.

1. **프로젝트 옵션 > iOS IPA 옵션> iTunesArtwork 이미지 포함** 및 **임시/엔터프라이즈 패키지(IPA) 빌드**가 선택되어 있는지 확인합니다.

    ![](ipa-support-images/imagexs04.png "iTunesArtwork 이미지 포함 및 임시/엔터프라이즈 패키지(IPA) 빌드 선택 확인")

    대신, 원하는 경우 텍스트 편집기에서 **.csproj** 파일을 편집하고, 앱을 빌드하는 데 사용될 구성에 대한 두 개의 해당 속성을 `PropertyGroup`에 수동으로 추가할 수 있습니다.

    ```xml
    <BuildIpa>true</BuildIpa>
    <IpaIncludeArtwork>false</IpaIncludeArtwork>
    ```

1. 선택적 **iTunesMetadata.plist** 파일을 포함하는 경우 **...** 단추를 클릭하고, 목록에서 이 파일을 선택하고, **확인** 단추를 클릭합니다.

     ![](ipa-support-images/imagexs03.png "목록에서 iTunesMetadata.plist 선택")

1. **msbuild**를 직접 호출하고 명령줄에서 이 속성을 전달합니다.

    ```bash
    /Library/Frameworks/Mono.framework/Commands/msbuild YourSolution.sln /p:Configuration=Ad-Hoc /p:Platform=iPhone /p:BuildIpa=true
    ```

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

프로비전 프로필이 만들어지고 선택되면 선택적 **iTunesMetadata.plist** 파일이 만들어지고, Visual Studio에서 iTunes 아트워크가 설정되면 배포용 IPA를 빌드할 수 있습니다. 다음으로 프로젝트를 구성해야 합니다. 다음을 수행합니다.

1. 편집하기 위해 **솔루션 탐색기**에서 Xamarin.iOS 프로젝트 이름을 마우스 오른쪽 단추로 클릭하고 **속성**을 선택하여 엽니다.

    ![](ipa-support-images/imagevs01.png "속성 선택")

2. **iOS IPA 옵션**을 선택하고 **구성** 드롭다운 목록에서 **임시**를 선택합니다.

    ![](ipa-support-images/imagevs02.png "구성 드롭다운 목록에서 임시 선택")

    > [!NOTE]
    > 최신 Xamarin.iOS 프로젝트에는 임시 구성을 사용할 수 없습니다. 이 경우 **릴리스** 구성을 대신 선택합니다.

3. **iTunesMetadata.plist** 파일을 포함하는 경우 **...** 단추를 클릭하고, 목록에서 이 파일을 선택하고, **열기** 단추를 클릭합니다.

    ![](ipa-support-images/imagevs03.png "목록에서 iTunesMetadata.plist 선택")

4. 필요에 따라 IPA에 대해 **패키지 이름**을 지정할 수 있습니다. 지정하지 않으면 Xamarin.iOS 프로젝트와 동일한 이름이 적용됩니다.
5. 변경 내용을 프로젝트 속성에 저장합니다.
6. 사용 가능한 경우 **빌드 구성** 드롭다운에서 **임시**를 선택합니다. 그렇지 않으면 **릴리스**를 선택합니다.

    ![](ipa-support-images/imagevs05.png "빌드 구성 드롭다운에서 임시 선택")

7. 프로젝트를 빌드하여 IPA 패키지를 만듭니다.
8. IPA는 **Bin &gt; iOS 디바이스 &gt; 임시(또는 릴리스)** 폴더에 빌드됩니다.

    ![](ipa-support-images/imagevs06.png "파일 탐색기의 IPA")

-----

<a name="Customizing-the-IPA-Location" />

## <a name="customizing-the-ipa-location"></a>IPA 위치 사용자 지정

**.ipa** 파일 출력 위치를 쉽게 사용자 지정할 수 있도록 새로운 `IpaPackageDir` **MSBuild** 속성이 추가되었습니다. `IpaPackageDir`을 사용자 지정 위치로 설정하면 **.ipa** 파일이 기본 타임스탬프가 적용된 하위 디렉터리 대신 해당 위치에 배치됩니다. 이는 CI(지속적인 통합) 빌드에 사용되는 것과 같이 특정 디렉터리 경로를 올바르게 사용하도록 자동화된 빌드를 만들 때 유용할 수 있습니다.

새 속성을 사용하는 방법에는 여러 가지가 있습니다.

예를 들어 **.ipa** 파일을 이전의 기본 디렉터리(Xamarin.iOS 9.6 이하)로 출력하려면 다음 방법 중 하나를 사용하여 `IpaPackageDir` 속성을 `$(OutputPath)`로 설정할 수 있습니다. 두 방법은 모두 IDE 빌드 및 **msbuild**, **xbuild** 또는 **mdtool**을 사용하는 명령줄 빌드를 포함하여 모든 Unified API Xamarin.iOS 빌드와 호환됩니다.

- 첫 번째 옵션은 **MSBuild** 파일의 `<PropertyGroup>` 요소 내에 `IpaPackageDir` 속성을 설정하는 것입니다. 예를 들어 다음 `<PropertyGroup>`을 **.csproj** iOS 앱 프로젝트 파일의 아래쪽(닫은 `</Project>` 태그 바로 앞)에 추가할 수 있습니다.

    ```xml
    <PropertyGroup>
        <IpaPackageDir>$(OutputPath)</IpaPackageDir>
    </PropertyGroup>
    ```

- **.ipa** 파일을 빌드하는 데 사용되는 구성에 해당하는 기존 `<PropertyGroup>`의 아래쪽에 `<IpaPackageDir>` 요소를 추가하는 것이 더 좋습니다. 이는 iOS IPA 옵션 프로젝트 속성 페이지에서 계획된 설정과의 향후 호환성을 위해 프로젝트를 준비하기 때문입니다. 현재 `Release|iPhone` 구성을 사용하여 **.ipa** 파일을 빌드하는 경우 업데이트된 속성 그룹 전체는 다음과 비슷할 수 있습니다.

    ```xml
    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhone' ">
        <Optimize>true</Optimize>
        <OutputPath>bin\iPhone\Release</OutputPath>
        <ErrorReport>prompt</ErrorReport>
        <WarningLevel>4</WarningLevel>
        <ConsolePause>false</ConsolePause>
        <CodesignKey>iPhone Developer</CodesignKey>
        <MtouchUseSGen>true</MtouchUseSGen>
        <MtouchUseRefCounting>true</MtouchUseRefCounting>
        <MtouchFloat32>true</MtouchFloat32>
        <CodesignEntitlements>Entitlements.plist</CodesignEntitlements>
        <MtouchLink>SdkOnly</MtouchLink>
        <MtouchArch>;ARMv7, ARM64</MtouchArch>
        <MtouchHttpClientHandler>HttpClientHandler</MtouchHttpClientHandler>
        <MtouchTlsProvider>Default</MtouchTlsProvider>
        <PlatformTarget>x86&</PlatformTarget>
        <BuildIpa>true</BuildIpa>
        <IpaPackageDir>$(OutputPath</IpaPackageDir>
    </PropertyGroup>
    ```

**msbuild** 또는 **xbuild** 명령줄 빌드에 대한 또 다른 방법은 `/p:` 인수를 추가하여 `IpaPackageDir` 속성을 설정하는 것입니다. 이 경우 **msbuild**는 명령줄에서 전달된 `$()` 식을 확장하지 않으므로 `$(OutputPath)` 구문을 사용할 수 없습니다. 대신 전체 경로 이름을 제공해야 합니다. Mono의 **xbuild** 명령은 `$()` 식을 확장하지만 [플랫폼 간 버전의 **msbuild**](https://www.mono-project.com/docs/about-mono/releases/5.0.0/#msbuild)를 선호하여 **xbuild**는 사용 중단되었므로 전체 경로 이름을 사용하는 것이 좋습니다.

이 방법을 사용하는 전체 예제는 Windows에서 다음과 비슷하게 보일 수 있습니다.

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser" /p:IpaPackageDir="%USERPROFILE%\Builds" /t:Build SingleViewIphone1.sln
```

또는 Mac에서 다음과 같습니다.

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:IpaPackageDir="$HOME/Builds" /t:Build SingleViewIphone1.sln
```

<a name="installipa" />

## <a name="installing-an-ipa-using-itunes"></a>iTunes를 사용하여 IPA 설치

결과 IPA 패키지는 테스트 사용자에게 전달되어 iOS 디바이스에 설치하거나 엔터프라이즈 배포용으로 제공될 수 있습니다. 선택된 방법에 관계없이 최종 사용자는 IPA 파일을 두 번 클릭하거나 열려 있는 iTunes 창으로 끌어서 Mac 또는 Windows PC의 iTunes 애플리케이션에 패키지를 설치합니다.

새 iOS 애플리케이션은 **My Apps** 섹션에 표시되며, 여기서 마우스 오른쪽 단추를 클릭하고 애플리케이션에 대한 정보를 얻을 수 있습니다.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

 ![](ipa-support-images/installxs01.png "내 애플리케이션 섹션의 새 iOS 애플리케이션")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

 ![](ipa-support-images/installvs01.png "내 애플리케이션 섹션의 새 iOS 애플리케이션")

-----

이제 사용자는 iTunes를 자신의 디바이스와 동기화하여 새 iOS 애플리케이션을 설치할 수 있습니다.

<a name="Summary" />

## <a name="summary"></a>요약

이 문서에서는 앱 스토어 이외의 빌드를 위한 Xamarin.iOS 애플리케이션을 준비하는 데 필요한 설정에 대해 설명했습니다. IPA 패키지를 만드는 방법 및 테스트 또는 사내 배포를 위해 최종 사용자의 iOS 디바이스에 결과 iOS 애플리케이션을 설치하는 방법을 보여 주었습니다.

## <a name="related-links"></a>관련 링크

- [App Store 배포](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [iTunes Connect에서 앱 구성](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [앱 스토어에 게시](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [사내 배포](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [임시 배포](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [iTunesMetadata.plist 파일](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [문제 해결](~/ios/deploy-test/troubleshooting.md)
- [iTunes 아트워크](~/ios/app-fundamentals/images-icons/app-icons.md#itunes)
- [엔터프라이즈 앱(Apple) 개발 및 배포](https://help.apple.com/xcode/mac/current/#/devba5e7054d)
- [엔터프라이즈 앱 배포(WWDC 비디오)](https://developer.apple.com/videos/play/wwdc2014/705/)
