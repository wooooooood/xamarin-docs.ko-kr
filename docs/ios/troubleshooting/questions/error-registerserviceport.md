---
title: iOS RegisterServicePort에 디자이너 오류가 발생 했습니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 929A0080-B126-4744-BF88-A4A1EFBB6CC2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 62d4a06c62bffb23566f4f59f42a0c980f417c45
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/04/2018
ms.locfileid: "30776718"
---
# <a name="ios-designer-error-with-registerserviceport"></a>iOS RegisterServicePort에 디자이너 오류가 발생 했습니다.

## <a name="sample-error"></a>샘플 오류
> System.AggregateException: 하나 이상의 오류가 발생 했습니다. System.SystemException--->: (com.xamarin.MTHosting.2a0b1, com.apple.PowerManagement.control) RegisterServicePort: 반환 된 커널:-308 (-308): 갑자기 (ipc/mig) 서버

## <a name="explanation"></a>설명
사용 하 여 오류 `RegisterServicePort` 위에 같은 비슷한 오류 메시지가 문제 스파이웨어/컴퓨터에서 맬웨어를 일반적으로 및 합니다. 고려 하십시오는 [이 버그 보고서에 대 한 주석](https://bugzilla.xamarin.com/show_bug.cgi?id=21907#c4) 에 대 한 링크와 함께 자세한 내용은 [Apple 포럼 토론](https://discussions.apple.com/thread/5596008) 가능한 감염을 제거 하는 방법에 있습니다. 

MacOS 응용 프로그램을 지원 하기 위해는 문제 진단에 개방 **콘솔** 내 모든 파일을 삭제 하 고는 **진단 보고서가 사용자** 섹션 [ http://screencast.com/t/y9i3NKcuMy ](http://screencast.com/t/y9i3NKcuMy)합니다. Mac 용 Visual Studio를 시작 하 고 디자이너를 사용 하려고 합니다. 디자이너를 초기화 실패 한 후이 섹션에는 새 로그 파일 표시, 분석 하기 위해 이러한 저장 하십시오.  

에 대 한 확인 할 가장 중요 한 점은이 파일 note 하십시오. 
> /usr/lib/libimckit.dylib

위의 결과 관계 없이 해당 파일이 있으면이 앞에서 언급 한 스파이웨어/맬웨어 문제는 사용자 컴퓨터에 존재 합니다.  

다음 링크를 통해이 스파이웨어/맬웨어를 제거 하는 단계에 있습니다. [http://www.thesafemac.com/arg-genieo/](http://www.thesafemac.com/arg-genieo/)  

