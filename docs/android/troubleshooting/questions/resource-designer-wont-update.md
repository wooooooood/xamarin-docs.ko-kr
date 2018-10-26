---
title: Android Resource.designer.cs 파일이 업데이트 되지 않습니다.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 3F7376E3-59CC-4722-AEED-BB50E4D952AA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/19/2017
ms.openlocfilehash: ba3c2b07e7f35bf9fd84d10b74d034a02ca6a73d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106418"
---
# <a name="my-android-resourcedesignercs-file-will-not-update"></a>Android Resource.designer.cs 파일이 업데이트 되지 않습니다.

> [!NOTE]
> Xamarin Studio 5.1.4 및 이상 버전에서이 문제가 해결 되었습니다. 그러나 Mac 용 Visual Studio에서 문제가 발생 하는 경우를 보내주세요를 [새 버그](~/cross-platform/troubleshooting/questions/howto-file-bug.md) 정보 및 전체 빌드 로그 출력 전체 버전을 사용 하 여 합니다.

Xamarin.Studio 5.1의 버그는 이전에 부분적으로 또는 완전히.csproj 파일의 xml 코드를 삭제 하 여.csproj 파일을 손상입니다. 이 인해 중요 한 부분 Android 빌드 시스템 (예: Android Resource.designer.cs 업데이트)에 실패 합니다. 안정적인 5.1.4 일부 터 7 월 15 일에 릴리스,이 버그가 수정 되었습니다. 하지만 대부분의 프로젝트 파일에 수동으로 아래 설명 된 대로 복구 해야 합니다.


## <a name="two-possible-approaches-to-fixing-up-the-project-file"></a>프로젝트 파일을 수정 하는 다음 두 가지 가능한 방법

### <a name="either"></a>다음 중 하나입니다.

1) 이전 프로젝트와 일치 프로그램 리소스, 소스 파일 등의 모든 프로젝트에 다시 추가 하는 모든 프로젝트 속성을 설정, 새로운 Xamarin.Android 응용 프로그램 프로젝트를 만듭니다.

### <a name="or"></a>또는

2) 원래 프로젝트의.csproj 파일의 백업 복사본, 텍스트 편집기에서 엽니다 및 완전히 생성 된.csproj 파일에서 누락 된 요소를 다시 추가 합니다.

### <a name="if-this-does-not-solve-the-problem"></a>이 문제가 해결 되지 않는 경우

이러한 요소를 사용 하 여 실험을 한 후 표시 될 수도 있습니다는 Resource.designer.cs 파일이 업데이트는 다시 요소를 추가 하 고 프로젝트를 다시 빌드한 후 다음 계속 해야를 닫고 다시 인식 코드 완성 기능을 가져오려면 솔루션을 엽니다. Resource.designer.cs에 포함 된 새 형식입니다. 
