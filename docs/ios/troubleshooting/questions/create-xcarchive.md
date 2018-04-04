---
title: Visual Studio에서.xcarchive 보관을 만들 수는?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 417D84FB-1BA9-4DB9-A683-66E960BA3D0D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 23af3bf277ab68ffe93df2e1ee8ee64afc01828d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="is-it-possible-to-create-a-xcarchive-archive-from-visual-studio"></a>Visual Studio에서.xcarchive 보관을 만들 수는?

## <a name="for-xamarin-4"></a>Xamarin 4에 대 한

Xamarin을 기준으로 4.x 되었습니다 만들 수는 `.xcarchive` 설정 하 여 Windows에서의 `ArchiveOnBuild` 속성을 `true`합니다. 예를 들어,를 사용 하 여 `MSBuild` 명령줄에서:

```bash
msbuild /p:Configuration=Release /p:ServerAddress=10.211.55.2 /p:ServerUser=xamUser /p:Platform=iPhone /p:ArchiveOnBuild=true /t:"Build" MyProject.csproj
```

`.xcarchive` 에 배치 됩니다는 `$HOME/Library/Developer/Xcode/Archives` Xcode 및 Xamarin Studio를 모두 표시 이전에 작성 된 보관 파일을 검색 하는 Mac 빌드 호스트의 디렉터리입니다.

이 참조 [Xamarin 포럼 게시물](https://forums.xamarin.com/discussion/comment/156635/#Comment_156635) 에 대 한 추가 참고 사항에 대 한 간단한 일부에 대 한는 `ArchiveOnBuild` 속성입니다. 에 대 한 설명서를 참조 하십시오. [Windows에 Xamarin.iOS 명령줄 빌드](~/ios/get-started/installation/windows/connecting-to-mac/index.md) 에 대 한 자세한 내용은 `ServerAddress` 및 `ServerUser` 속성입니다.

* * *

## <a name="for-xamarin-3-and-earlier"></a>3 및 이전 버전의 Xamarin에 대 한

Xamarin 3.x Visual Studio 확장 생성 하는 메커니즘을 제공 하지 않습니다 `.xcarchive` 보관 합니다. 즉를 만드는 데 사용 하는 논리 `.xcarchive` Mac에서 Xamarin Studio에서 보관 [여기에 설명 된](https://bugzilla.xamarin.com/show_bug.cgi?id=35#c5)이므로 만들 수 있습니다 아마도 사용자 고유의 `.xcarchive` 직접 받으려는 경우에 유용 합니다.

필요 하지 않습니다 주목할 하지만 `.xcarchive` 앱 스토어에 제출 합니다. 앱 스토어 분포 프로필 (한 임시 배포 프로필 아님)로 서명 되어으로 IPA 파일을 제출할 수 있습니다.

사실, 있습니다 수 심지어을 압축는 `.app` (응용 프로그램 저장 분포 프로필 서명) 된 번들 및 제출 하는 `.zip` 앱 스토어에 대 한 파일입니다.

두 경우 모두는 응용 프로그램 (아님 Xcode)를 제출 하려면 응용 프로그램 로더 응용 프로그램을 사용할 수 있습니다.

