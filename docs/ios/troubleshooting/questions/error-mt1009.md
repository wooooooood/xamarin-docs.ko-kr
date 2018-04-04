---
title: '오류 MT1009: 어셈블리를 복사할 수 없습니다.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F9FEDFF5-C84C-42B4-8F25-E34846E7315A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: de1d7ea8ce4c358ad69150be3da6b38fb6c730ae
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
---
# <a name="error-mt1009-could-not-copy-the-assembly"></a>오류 MT1009: 어셈블리를 복사할 수 없습니다.

> [!IMPORTANT]
> Xamarin.iOS의 최신 버전에이 문제가 해결 되었습니다. 그러나 최신 버전의 소프트웨어에 문제가 발생 하면 보관는 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 정보 및 전체 빌드 로그 출력 전체 프로그램 버전 관리를 사용 합니다.

에 설명 된 대로 우리의 [릴리스 정보](https://developer.xamarin.com/releases/ios/xamarin.ios_7/xamarin.ios_7.2/), Xamarin.iOS 7.2.6의에서 알려진된 문제입니다. 이 문제는 개발자의 주요 계정을 다음 파일 사용 권한을 다른 사용자 계정으로 Xamarin.iOS 설치 될 때 더 높은 권한이 필요한 때문입니다.

이 문제를 해결 하려면 하십시오 Mac 워크스테이션 Terminal.app 열고 다음 명령을 실행 합니다.

`sudo chmod 0644 /Developer/MonoTouch/usr/lib/mono/2.1/monotouch.dll.mdb`

