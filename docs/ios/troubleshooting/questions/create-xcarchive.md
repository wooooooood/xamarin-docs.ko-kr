---
title: Visual Studio에서 .xcarchive 보관 파일을 만들 수 있나요?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 417D84FB-1BA9-4DB9-A683-66E960BA3D0D
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: 1b078b8cb4d1129127997e9fabdd0b128e09c90f
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70769362"
---
# <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studio"></a>Visual Studio에서 .xcarchive 보관 파일을 만들 수 있나요?

## <a name="for-xamarin-4"></a>Xamarin 4의 경우

Xamarin 4.x를 기준으로 이제 `.xcarchive` `ArchiveOnBuild` 속성을로 `true`설정 하 여 Windows에서을 만들 수 있습니다. 예를 들어 명령줄 `MSBuild` 에서를 사용 합니다.

```bash
msbuild /p:Configuration=Release /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhone /p:ArchiveOnBuild=true /t:"Build" MyProject.csproj
```

는 `.xcarchive` Mac 빌드 호스트의 `$HOME/Library/Developer/Xcode/Archives` 디렉터리에 배치 됩니다 .이 디렉터리는 Xcode 및 Xamarin Studio 검색을 모두 통해 이전에 빌드한 보관을 표시 합니다.

`ArchiveOnBuild` 속성에 대 한 몇 가지 간단한 추가 정보는이 [Xamarin 포럼 게시물](https://forums.xamarin.com/discussion/comment/156635/#Comment_156635) 을 참조 하세요. `ServerAddress` 및 `ServerUser` 속성에 대한 자세한 내용은 [Windows의 xamarin.ios 명령줄 빌드](~/ios/get-started/installation/windows/connecting-to-mac/index.md)에 대한 설명서를 참조하세요.

* * *

## <a name="for-xamarin-3-and-earlier"></a>Xamarin 3 및 이전 버전의 경우

Xamarin 3(sp3)의 Visual Studio 확장은 보관 파일을 생성 `.xcarchive` 하는 메커니즘을 제공 하지 않습니다. 즉, Mac에서 Xamarin Studio에 보관 파일 `.xcarchive` 을 만드는 데 사용 되는 논리는 [여기에서 설명](https://bugzilla.xamarin.com/show_bug.cgi?id=35#c5)하므로 원하는 경우 직접 직접 `.xcarchive` 만들 수 있습니다.

하지만 앱 스토어에 제출 하기 위해가 `.xcarchive` 필요 하지는 않습니다. IPA 파일은 임시 배포 프로필이 아닌 앱 스토어 배포 프로필을 사용 하 여 서명 된 경우에만 제출할 수 있습니다.

실제로 `.app` 번들 (앱 스토어 배포 프로필을 사용 하 여 서명 됨)을 zip 하 고 해당 `.zip` 파일을 앱 스토어에 전송할 수도 있습니다.

어떤 경우 든 응용 프로그램 로더 앱을 사용 하 여 앱을 제출할 수 있습니다 (Xcode 대신).
