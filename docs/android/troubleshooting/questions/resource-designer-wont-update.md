---
title: "Android Resource.designer.cs 파일이 업데이트 되지 않습니다."
ms.topic: article
ms.prod: xamarin
ms.assetid: 3F7376E3-59CC-4722-AEED-BB50E4D952AA
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/19/2017
ms.openlocfilehash: b169bcc64af15de3d87bfb7f8059b4251f1a3ad9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/27/2018
---
# <a name="my-android-resourcedesignercs-file-will-not-update"></a>Android Resource.designer.cs 파일이 업데이트 되지 않습니다.

> [!NOTE]
> **참고:** Xamarin Studio 5.1.4 이상 버전에서이 문제가 해결 되었습니다. 그러나 Mac 용 Visual Studio에서 문제가 발생 하는 경우 보관는 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 정보 및 전체 빌드 로그 출력 전체 프로그램 버전 관리를 사용 합니다.

이전에 Xamarin.Studio 5.1의 버그는 부분적으로 또는 완전히.csproj 파일의 xml 코드를 삭제 하 여.csproj 파일 손상 되었습니다. 이 인해 시스템 (예: Android Resource.designer.cs 업데이트)을 작성 하는 Android의 일부 중요 한 실패 합니다. 7 월 15 일에 발표 안정적인 5.1.4 기준으로,이 버그는 수정 되었습니다. 하지만 대부분의 경우 프로젝트 파일에 수동으로 아래 설명 된 대로 복구 해야 합니다.


## <a name="two-possible-approaches-to-fixing-up-the-project-file"></a>프로젝트 파일을 수정 하는 두 가지 가능한 방법

### <a name="either"></a>다음 중 하나입니다.

1) 새로운 Xamarin.Android 응용 프로그램 프로젝트를 만들고 이전 프로젝트와 일치 하면 리소스, 소스 파일 등의 모든 프로젝트에 다시 추가 하는 모든 프로젝트 속성을 설정 합니다.

### <a name="or"></a>또는

2) 원본 프로젝트의.csproj 파일의 백업 복사본 만들기, 텍스트 편집기에서 엽니다 및 데이터베이스가 완전히 생성 된.csproj 파일에서 누락 된 요소에 다시 추가 합니다.

### <a name="if-this-does-not-solve-the-problem"></a>이 문제가 해결 되지 않는 경우

이러한 요소를 실험 한 후 없는데 다시 요소를 추가 하 고 프로젝트를 다시 Resource.designer.cs 파일은 업데이트할 수 있지만 다음 계속 해야 닫았다가 다시 열어서 코드 완성 인식 하도록 솔루션은 후 Resource.designer.cs에 포함 된 새 형식입니다. 
