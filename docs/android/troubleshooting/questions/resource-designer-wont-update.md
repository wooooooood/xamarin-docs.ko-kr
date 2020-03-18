---
title: 내 Android Resource.designer.cs 파일이 업데이트되지 않습니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3F7376E3-59CC-4722-AEED-BB50E4D952AA
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/19/2017
ms.openlocfilehash: 4683bbaa5aa48c7b5de5fb9a87a4cd3fbc0aeada
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/10/2020
ms.locfileid: "73019507"
---
# <a name="my-android-resourcedesignercs-file-will-not-update"></a>내 Android Resource.designer.cs 파일이 업데이트되지 않습니다.

> [!NOTE]
> 이 문제는 Xamarin Studio 5.1.4 이상 버전에서 해결되었습니다. 그러나 Mac용 Visual Studio에서 문제가 발생하는 경우 전체 버전 정보 및 전체 빌드 로그 출력으로 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md)를 제출합니다.

Xamarin.Studio 5.1의 버그는 .csproj 파일에서 xml 코드를 부분적으로 또는 완전히 삭제하여 이전에 .csproj 파일을 손상시켰습니다. 이로 인해 Android 빌드 시스템의 중요한 부분(예: Android Resource.designer.cs 업데이트)이 실패할 수 있습니다. 7월 15일 5.1.4 안정적인 릴리스를 기준으로 이 버그가 수정되었지만, 대부분의 경우 아래 설명된 대로 프로젝트 파일을 수동으로 복구해야 합니다.

## <a name="two-possible-approaches-to-fixing-up-the-project-file"></a>프로젝트 파일을 수정하는 두 가지 방법을 사용할 수 있습니다.

**다음 중 하나입니다.**

1. 새 Xamarin.Android 애플리케이션 프로젝트를 만들어 이전 프로젝트와 일치하도록 모든 프로젝트 속성을 설정하고 모든 리소스, 소스 파일 등을 프로젝트에 다시 추가합니다.

   **OR**

2. 원본 프로젝트의 .csproj 파일 백업 복사본을 만든 다음, 텍스트 편집기에서 열고 완전히 생성된 .csproj 파일에서 누락된 요소를 다시 추가합니다.

### <a name="if-this-does-not-solve-the-problem"></a>그래도 문제가 해결되지 않는 경우

이러한 요소를 실험한 후 요소를 다시 추가하고 프로젝트를 다시 빌드한 후에 Resource.designer.cs 파일이 업데이트되지만, Resource.designer.cs에 포함된 새로운 유형을 인식하기 위해 코드 완성을 가져오려면 솔루션을 닫았다가 다시 열어야 할 수도 있습니다. 
