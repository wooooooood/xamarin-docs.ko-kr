---
title: 애플리케이션 게시
ms.prod: xamarin
ms.assetid: 51E19000-040A-2B74-C462-EC57C617085C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: fe1422aa55e5c1518134e6d0fbbf40047b577767
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70753803"
---
# <a name="publishing-an-application"></a>애플리케이션 게시

멋진 애플리케이션을 만들었다면 사람들이 이를 사용하고자 할 것입니다. 이 섹션에서는 Xamarin.Android로 만든 애플리케이션을 이메일, 개인 웹 서버, Google Play 또는 Amazon App Store for Android 등을 통해 공개 배포하는 단계를 설명합니다.

## <a name="overview"></a>개요

Xamarin.Android 애플리케이션 개발의 마지막 단계는 애플리케이션을 게시하는 것입니다. 게시는 Xamarin.Android 애플리케이션을 컴파일하여 사용자가 디바이스에 설치할 수 있게 하는 프로세스로, 두 가지 필수 작업이 관여합니다.

- **게시 준비** &ndash; Android 지원 디바이스에 배포할 수 있는 애플리케이션의 릴리스 버전이 만들어집니다(릴리스 준비에 대한 자세한 내용은 [애플리케이션 릴리스 준비](~/android/deploy-test/release-prep/index.md) 참조).

- **배포** &ndash; 애플리케이션의 릴리스 버전을 다양한 배포 채널 중 하나 이상을 통해 제공합니다.

다음 다이어그램에서는 Xamarin.Android 애플리케이션 게시와 관련된 단계를 보여 줍니다.

[![빌드 및 배포 순서도](images/build-and-deploy-steps.png)](images/build-and-deploy-steps.png#lightbox)

위의 다이어그램에서 볼 수 있듯이 사용되는 배포 방법에 관계없이 준비는 동일합니다. 몇 가지 방법으로 Android 애플리케이션을 사용자에게 릴리스할 수 있습니다.

- **웹 사이트를 통해**&ndash; Xamarin.Android 애플리케이션을 웹 사이트의 다운로드로 제공할 수 있습니다. 여기서 사용자가 링크를 클릭하여 애플리케이션을 설치하게 됩니다.
- **이메일을 통해**&ndash; 사용자가 자신의 이메일에서 Xamarin.Android 애플리케이션을 설치할 수 있습니다. Android 지원 디바이스로 첨부 파일을 열면 애플리케이션이 설치됩니다.
- **마켓을 통해**&ndash;[Google Play](http://play.google.com/) 또는 [Amazon App Store for Android](http://www.amazon.com/mobile-apps/b?ie=UTF8&node=2350149011) 등, 배포를 위한 몇 가지 애플리케이션 마켓플레이스가 있습니다.

기존 마켓플레이스는 광범위한 시장 접근과 최대 규모의 배포 관리를 제공하므로 가장 일반적인 애플리케이션 게시 방법입니다. 그러나 마켓플레이스를 통해 애플리케이션을 게시하려면 추가적인 작업이 필요합니다.

여러 채널이 Xamarin.Android 애플리케이션을 동시에 배포할 수 있습니다. 예를 들어 애플리케이션을 Google Play, Amazon App Store for Android에 게시하고 웹 서버에서 다운로드할 수도 있습니다.

다른 두 배포 방법(다운로드 또는 이메일)은 기업 환경이나 소규모의 특정한 사용자 전용 애플리케이션 등, 관리되는 사용자 집단에서 가장 유용합니다.
서버와 이메일 배포도 더 간단한 게시 모델로, 애플리케이션 게시에 준비 작업이 덜 소요됩니다.

Amazon Mobile App Distribution Program을 사용하면 모바일 앱 개발자들이 애플리케이션을 Amazon에서 배포 및 판매할 수 있습니다. 사용자들은 Amazon App Store 애플리케이션을 사용하여 Android 지원 디바이스에서 앱을 검색 및 쇼핑할 수 있습니다. Android 디바이스에서 실행되는 Amazon App Store의 스크린 샷은 다음과 같습니다.

Google Play는 Android 애플리케이션에 대한 가장 포괄적이고 인기 있는 마켓플레이스입니다. Google Play에서는 사용자가 디바이스나 컴퓨터의 간단한 아이콘을 클릭하여 애플리케이션을 검색, 다운로드, 평가 및 결제할 수 있습니다. Google Play는 판매 및 시장 추세 분석을 지원하고 애플리케이션을 다운로드할 수 있는 디바이스 및 사용자를 제어하는 도구도 제공합니다. Android 디바이스에서 실행되는 Google Play의 스크린 샷은 다음과 같습니다.

[![Google Play 스크린샷](images/google-play-app.png)](images/google-play-app.png#lightbox)

이 섹션에서는 적합한 프로모션 자료와 함께 Google Play 등의 스토어에 애플리케이션을 업로드하는 방법을 보여 줍니다. APK 확장 파일이 무엇이며 어떻게 작동하는지를 개념적으로 설명합니다. Google 라이선스 서비스에 대해서도 설명합니다. 마지막으로 HTTP 웹 서버, 간단한 이메일 배포 및 Amazon App Store for Android 등, 다른 배포 방법에 대해서 소개합니다.

## <a name="related-links"></a>관련 링크

- [HelloWorldPublishing(샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/helloworldpublishing)
- [빌드 프로세스](~/android/deploy-test/building-apps/build-process.md)
- [링크](~/android/deploy-test/linker.md)
- [Google Maps API 키 가져오기](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [애플리케이션 서명](https://source.android.com/security/apksigning/)
- [Google Play에 게시](https://developer.android.com/distribute/googleplay/publish/index.html)
- [Google 애플리케이션 라이선스](https://developer.android.com/guide/google/play/licensing/index.html)
- [Android.Play.ExpansionLibrary](https://github.com/mattleibow/Android.Play.ExpansionLibrary)
- [모바일 앱 배포 포털](https://developer.amazon.com/welcome.html)
- [Amazon 모바일 앱 배포 FAQ](https://developer.amazon.com/help/faq.html)
