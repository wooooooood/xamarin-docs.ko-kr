---
title: Xamarin.Mac 용 Mac 라이브러리 바인딩
description: 이 문서는 목표 Sharpie 및 샘플 코드를 포함 하 여 Xamarin.Mac 응용 프로그램에서는 Objective-c 바인딩 작업을 수행 하는 방법에 설명 하는 지침에 연결 합니다.
ms.prod: xamarin
ms.assetid: 521707CD-79D3-488A-84CB-A37EBF93AC94
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 01/13/2017
ms.openlocfilehash: fde21b2056d56cbf1c4768b287e29f559390f500
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107302"
---
# <a name="binding-mac-libraries-for-xamarinmac"></a>Xamarin.Mac 용 Mac 라이브러리 바인딩

Xamarin.Mac의 Objective-c 라이브러리 바인딩 방법을 알아보려면 다음이 링크를 수행 합니다.

- [**개요** ](~/cross-platform/macios/binding/overview.md) -
  바인딩이 작동 하는 방법에 대해 설명 합니다.
- [**Objective-c 라이브러리 바인딩** ](~/cross-platform/macios/binding/objective-c-libraries.md) -
  Xamarin 프로젝트에서 사용 하기 위한 Objective-c 라이브러리를 바인딩하는 방법에 대 한 지침입니다.
- [**입력 정의 참조 가이드** ](~/cross-platform/macios/binding/binding-types-reference.md) -
  모든 바인딩 생성 프로세스를 추진 하는 바인딩 작성자가 사용할 수 있는 특성에 설명 합니다.

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

목표 Sharpie는 바인딩의 첫 번째 패스를 부트스트랩 하는 데는 명령줄 도구입니다.
공용 API에 매핑할 네이티브 라이브러리의 헤더 파일을 구문 분석 하 여 작동 합니다 [바인딩 정의](~/cross-platform/macios/binding/binding-types-reference.md) (그렇지 않은 경우 수동으로 수행 되는 프로세스). 목표 Sharpie 자체적으로 바인딩을 만들지 않습니다 하지만 시작 하는 데 도움이!

## <a name="examples"></a>예제

참조 된 [XMBindingExample Mac 샘플](https://github.com/xamarin/mac-samples/tree/master/XMBindingExample) 바인딩 프로젝트를 사용 하 여 Mac 바인딩을 만드는 방법.

## <a name="related-links"></a>관련 링크

- [Objective-C 바인딩](~/cross-platform/macios/binding/index.md)
- [IOS 라이브러리 바인딩](~/ios/platform/binding-objective-c/index.md)
