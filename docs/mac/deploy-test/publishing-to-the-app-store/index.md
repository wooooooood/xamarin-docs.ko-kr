---
title: Mac App Store에 Xamarin.Mac 앱 게시
description: 이 문서에서는 Mac용 Visual Studio를 사용하여 Xamarin.Mac 앱을 배포하는 방법을 설명합니다. Mac 개발자 계정을 설정하는 방법, 코드 서명을 위해 인증서를 만드는 방법 및 이를 사용하여 직접 또는 Mac App Store를 통해 배포할 수 있는 Mac 앱을 빌드하는 방법을 설명합니다.
ms.prod: xamarin
ms.assetid: D26C5E54-EAD2-5487-264D-4263AEA1EBF2
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 601e8f0e47c07aab6d3c56b4799716e10ec606ab
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105872"
---
# <a name="publishing-xamarinmac-apps-to-the-mac-app-store"></a>Mac App Store에 Xamarin.Mac 앱 게시

## <a name="overview"></a>개요

Xamarin.Mac 앱은 두 가지 방법으로 배포할 수 있습니다.

- **개발자 ID** – 개발자 ID로 서명된 응용 프로그램은 앱 스토어 외부에 배포할 수 있지만, 게이트키퍼에서 인식하고 설치할 수 있습니다.
- **Mac 앱 스토어** – 앱에는 설치 관리자 패키지가 있어야 하며, Mac 앱 스토어에 제출할 수 있도록 앱과 설치 관리자가 모두 서명되어야 합니다.

이 문서에서는 Mac용 Visual Studio 및 Xcode를 사용하여 Apple 개발자 계정을 설정하고 각 배포 유형에 대한 Xamarin.Mac 프로젝트를 구성하는 방법을 설명합니다.


## <a name="mac-developer-program"></a>Mac 개발자 프로그램

[Mac 개발자 프로그램](https://developer.apple.com/devcenter/mac/)에 가입하는 개발자는 아래 스크린샷처럼 개인으로 가입할 것인지 아니면 회사로 가입할 것인지 선택할 수 있습니다.

[![Apple 개발자 포털](images/image1.png "Apple 개발자 포털")](images/image1-large.png#lightbox)

상황에 맞는 올바른 등록 형식을 선택합니다.

> [!NOTE]
> 여기서 선택하는 내용은 개발자 계정을 구성할 때 표시되는 일부 화면에 영향을 줍니다. 이 문서의 설명과 스크린샷은 **개인** 개발자 계정의 관점에서 수행됩니다. **회사** 계정의 경우 일부 옵션은 **팀 관리자** 사용자에게만 제공됩니다.


### <a name="certificates-and-identifiersmacdeploy-testpublishing-to-the-app-storecertificates-identifiersmd"></a>[인증서 및 식별자](~/mac/deploy-test/publishing-to-the-app-store/certificates-identifiers.md)

이 가이드에서는 Xamarin.Mac 앱을 게시하는 데 필요한 인증서 및 식별자를 만드는 방법을 안내합니다.


### <a name="create-provisioning-profilemacdeploy-testpublishing-to-the-app-storeprofilesmd"></a>[프로비전 프로필 만들기](~/mac/deploy-test/publishing-to-the-app-store/profiles.md)

이 가이드에서는 Xamarin.Mac 앱을 게시하는 데 필요한 프로비전 프로필을 만드는 방법을 안내합니다.


### <a name="mac-app-configurationmacdeploy-testpublishing-to-the-app-storeapp-configurationmd"></a>[Mac 앱 구성](~/mac/deploy-test/publishing-to-the-app-store/app-configuration.md)

이 가이드에서는 게시할 Xamarin.Mac 앱을 구성하는 방법을 안내합니다.


### <a name="sign-with-developer-idmacdeploy-testpublishing-to-the-app-storesigningmd"></a>[개발자 ID로 서명](~/mac/deploy-test/publishing-to-the-app-store/signing.md)

이 가이드에서는 게시할 Xamarin.Mac 앱을 개발자 ID로 서명하는 방법을 안내합니다.


### <a name="bundle-for-mac-app-storemacdeploy-testpublishing-to-the-app-storebundlingmd"></a>[Mac 앱 스토어용 번들](~/mac/deploy-test/publishing-to-the-app-store/bundling.md)

이 가이드에서는 Mac 앱 스토어에 게시할 Xamarin.Mac 앱을 묶는 방법을 안내합니다.


### <a name="upload-to-mac-app-storemacdeploy-testpublishing-to-the-app-storeuploadingmd"></a>[Mac 앱 스토어에 업로드](~/mac/deploy-test/publishing-to-the-app-store/uploading.md)

이 가이드에서는 Mac 앱 스토어에 게시할 Xamarin.Mac 앱을 업로드하는 방법을 안내합니다.


## <a name="related-links"></a>관련 링크

- [설치](/visualstudio/mac/installation/)
- [Hello, Mac 샘플](~/mac/get-started/hello-mac.md)
- [개발자 ID 및 게이트키퍼](https://developer.apple.com/resources/developer-id/)
