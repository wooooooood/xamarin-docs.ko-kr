---
title: RegisterServicePort에 발생하는 iOS 디자이너 오류
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: 68c87355a2a6a081e0fff741ffe8a4466abb540a
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292598"
---
# <a name="ios-designer-error-with-registerserviceport"></a>RegisterServicePort에 발생하는 iOS 디자이너 오류

## <a name="sample-error"></a>샘플 오류
> System.AggregateException: 하나 이상의 오류가 발생 했습니다 > System.web 예외가 발생 했습니다---. RegisterServicePort(com.xamarin.MTHosting.2a0b1, com.apple.PowerManagement.control): 반환 된 커널:-308 (-308): (ipc/하거나) 서버가 끊겼습니다.

## <a name="explanation"></a>설명
위와 유사한 `RegisterServicePort` 오류 메시지와 함께 발생 하는 오류는 일반적으로 컴퓨터의 스파이웨어/맬웨어에 문제가 있는 경우에 발생 합니다. 가능한 감염을 제거 하는 [방법에 대 한 자세한](https://discussions.apple.com/thread/5596008) 내용은 [이 버그 보고서의 설명을](https://bugzilla.xamarin.com/show_bug.cgi?id=21907#c4) 참조 하세요. 

문제를 진단 하는 데 도움이 되도록 macOS 응용 프로그램 **콘솔** 을 열고 **사용자 진단 보고서** 섹션 [http://screencast.com/t/y9i3NKcuMy](http://screencast.com/t/y9i3NKcuMy)내의 모든 파일을 삭제 합니다. 그런 다음 Mac용 Visual Studio를 시작 하 고 디자이너를 사용 하려고 합니다. 디자이너를 초기화 하지 못한 후이 섹션에 새 로그 파일이 표시 되 면 분석을 위해 저장 하세요.  

확인할 가장 중요 한 사항은 다음 파일 인지 확인 하십시오. 
> /usr/lib/libimckit.dylib

위의 결과에 관계 없이 해당 파일이 존재 하는 경우 앞에서 언급 한 스파이웨어/맬웨어 문제가 컴퓨터에 표시 됩니다.  

다음 링크에는이 스파이웨어/맬웨어를 제거 하는 단계가 있습니다.[http://www.thesafemac.com/arg-genieo/](http://www.thesafemac.com/arg-genieo/)  

