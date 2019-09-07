---
title: 내 Android Resource.designer.cs 파일이 업데이트되지 않습니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3F7376E3-59CC-4722-AEED-BB50E4D952AA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/19/2017
ms.openlocfilehash: c175c533e437736849eac6be1f4a90a783123812
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757144"
---
# <a name="my-android-resourcedesignercs-file-will-not-update"></a>내 Android Resource.designer.cs 파일이 업데이트되지 않습니다.

> [!NOTE]
> 이 문제는 Xamarin Studio 5.1.4 이상 버전에서 해결 되었습니다. 그러나 Mac용 Visual Studio에서 문제가 발생 하는 경우 전체 버전 정보 및 전체 빌드 로그 출력을 사용 하 여 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 를 작성 하세요.

.Csproj 파일에서 xml 코드를 부분적으로 또는 완전히 삭제 하 여 이전에 손상 된 .csproj 파일의 버그를 5.1 합니다. 이로 인해 android 빌드 시스템의 중요 한 부분 (예: Android Resource.designer.cs 업데이트)이 실패할 수 있습니다. 7 월 15 일에 5.1.4 안정적인 릴리스를 기준으로이 버그가 수정 되었습니다. 하지만 대부분의 경우 아래 설명 된 대로 프로젝트 파일은 수동으로 복구 해야 합니다.

## <a name="two-possible-approaches-to-fixing-up-the-project-file"></a>프로젝트 파일을 수정 하는 두 가지 방법을 사용할 수 있습니다.

**또는**

1. 새 Xamarin Android 응용 프로그램 프로젝트를 만들고, 이전 프로젝트와 일치 하도록 모든 프로젝트 속성을 설정 하 고, 모든 리소스, 소스 파일 등을 다시 프로젝트에 추가 합니다.

   **OR**

2. 원본 프로젝트의 .csproj 파일의 백업 복사본을 만든 다음 텍스트 편집기에서 열고 완전히 생성 된 .csproj 파일에서 누락 된 요소를 다시 추가 합니다.

### <a name="if-this-does-not-solve-the-problem"></a>그래도 문제가 해결 되지 않으면

이러한 요소를 실험 한 후에는 요소를 다시 추가 하 고 프로젝트를 다시 빌드한 후 Resource.designer.cs 파일이 업데이트 되지만, 코드 완성을 인식 하기 위해 솔루션을 닫았다가 다시 열어야 할 수도 있습니다. Resource.designer.cs에 포함 된 새 형식입니다. 
