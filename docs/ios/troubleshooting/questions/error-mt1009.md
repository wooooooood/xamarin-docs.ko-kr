---
title: '오류 MT1009: 어셈블리를 복사할 수 없습니다.'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F9FEDFF5-C84C-42B4-8F25-E34846E7315A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 4a67537cc53aeecf1b86d11dbf041cea79587dd2
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105407"
---
# <a name="error-mt1009-could-not-copy-the-assembly"></a>오류 MT1009: 어셈블리를 복사할 수 없습니다.

> [!IMPORTANT]
> 최신 버전의 Xamarin.iOS에서이 문제가 해결 되었습니다. 그러나 소프트웨어의 최신 버전에 문제가 발생 하면 제출 하세요를 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 정보 및 전체 빌드 로그 출력 전체 버전을 사용 하 여 합니다.

에 설명 된 대로 우리의 [릴리스](https://developer.xamarin.com/releases/ios/xamarin.ios_7/xamarin.ios_7.2/), 7.2.6 Xamarin.iOS의 알려진된 문제입니다. 이 문제는 다른 사용자 계정을 사용 하 여 Xamarin.iOS 설치 될 때 더 높은 권한이 필요로 하는 파일 권한으로 인해 다음 개발자의 기본 계정입니다.

문제를 해결 하려면 하십시오 Mac 워크스테이션 Terminal.app 열고 다음 명령을 실행 합니다.

`sudo chmod 0644 /Developer/MonoTouch/usr/lib/mono/2.1/monotouch.dll.mdb`

