---
title: Visual Studio에서.xcarchive 아카이브를 만들 수는?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 417D84FB-1BA9-4DB9-A683-66E960BA3D0D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/03/2018
ms.openlocfilehash: 1c20534e1d4476ce7ff85d9ffd5ae8742c445983
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350212"
---
# <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studio"></a>Visual Studio에서.xcarchive 아카이브를 만들 수는?

## <a name="for-xamarin-4"></a>Xamarin 4

Xamarin을 기준으로 4.x 아니며 이제 만들 수는 `.xcarchive` 설정 하 여 Windows에서의 `ArchiveOnBuild` 속성을 `true`입니다. 예를 들어,를 사용 하 여 `MSBuild` 명령줄에서:

```bash
msbuild /p:Configuration=Release /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhone /p:ArchiveOnBuild=true /t:"Build" MyProject.csproj
```

합니다 `.xcarchive` 에 배치 됩니다는 `$HOME/Library/Developer/Xcode/Archives` 표시 이전에 빌드한 보관 Xcode 및 Xamarin Studio를 모두 검색 하는 Mac 빌드 호스트에 디렉터리입니다.

이 참조 하세요 [Xamarin 포럼 게시물](https://forums.xamarin.com/discussion/comment/156635/#Comment_156635) 에 대 한 추가 정보에 대 한 간단한 일부에 대 한는 `ArchiveOnBuild` 속성입니다. 에 대 한 설명서를 참조 하세요 [Windows에 Xamarin.iOS 명령줄 빌드](~/ios/get-started/installation/windows/connecting-to-mac/index.md) 대 한 자세한 내용은 합니다 `ServerAddress` 및 `ServerUser` 속성입니다.

* * *

## <a name="for-xamarin-3-and-earlier"></a>Xamarin 3 및 이전 버전

Xamarin 3.x Visual Studio 확장을 생성 하는 메커니즘을 제공 하지 않습니다 `.xcarchive` 보관 합니다. 즉, 논리를 만드는 데 `.xcarchive` Mac에서 Xamarin Studio에서 보관 [여기에 설명 된](https://bugzilla.xamarin.com/show_bug.cgi?id=35#c5)이므로 만들 수 있습니다 아마도 사용자 고유의 `.xcarchive` 하려는 경우 수동으로.

필요 하지 않다는 주목할 만한 가치가 있지만 `.xcarchive` 앱 스토어에 제출 합니다. 앱 스토어 배포 프로필 (는 임시 배포 프로필 아님)를 사용 하 여 서명으로 IPA 파일을 제출할 수 있습니다.

사실, 있습니다 수 만으로도 압축 하는 `.app` (서명한 앱 저장 배포 프로필을 사용 하 여) 번들 및 제출 하는 `.zip` 앱 스토어에는 파일입니다.

두 경우 모두를 앱 대신 Xcode 전송할 응용 프로그램 로더 앱을 사용할 수 있습니다.

