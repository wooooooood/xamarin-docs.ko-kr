---
title: Xamarin.ios 용 Mac 라이브러리 바인딩
description: 이 문서는 목표 Sharpie 및 샘플 코드를 포함 하 여 Xamarin.ios 응용 프로그램에서 목표-C 바인딩 작업을 수행 하는 방법을 설명 하는 가이드로 연결 됩니다.
ms.prod: xamarin
ms.assetid: 521707CD-79D3-488A-84CB-A37EBF93AC94
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 01/13/2017
ms.openlocfilehash: 59ac5a4f9949f1e65e67b9629c43ddb4b822bf43
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290055"
---
# <a name="binding-mac-libraries-for-xamarinmac"></a>Xamarin.ios 용 Mac 라이브러리 바인딩

Xamarin.ios의 바인딩 목표-C 라이브러리에 대해 알아보려면 다음 링크를 참조 하세요.

- [**개요**](~/cross-platform/macios/binding/overview.md) -
  바인딩이 작동 하는 방식을 설명 합니다.
- [**바인딩 목표-C 라이브러리**](~/cross-platform/macios/binding/objective-c-libraries.md) -
  Xamarin 프로젝트에서 사용할 목적-C 라이브러리를 바인딩하는 방법에 대 한 지침입니다.
- [**형식 정의 참조 가이드**](~/cross-platform/macios/binding/binding-types-reference.md) -
  바인딩 생성 프로세스를 구동 하기 위해 바인딩 작성자가 사용할 수 있는 모든 특성을 설명 합니다.

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

목표 Sharpie는 첫 번째 바인딩 패스를 부트스트랩 하는 데 도움이 되는 명령줄 도구입니다.
네이티브 라이브러리의 헤더 파일을 구문 분석 하 여 공용 API를 [바인딩 정의](~/cross-platform/macios/binding/binding-types-reference.md) 에 매핑합니다 (수동으로 수행 하는 프로세스). 목표 Sharpie 자체 바인딩을 만들지 않지만 시작 하는 데 도움이 될 수 있습니다.

## <a name="examples"></a>예

바인딩 프로젝트를 사용 하 여 Mac 바인딩을 만드는 방법에 대 한 자세한 내용은 [Xmbindingexample Mac 샘플](https://github.com/xamarin/mac-samples/tree/master/XMBindingExample) 을 참조 하세요.

## <a name="related-links"></a>관련 링크

- [Objective-C 바인딩](~/cross-platform/macios/binding/index.md)
- [IOS 라이브러리 바인딩](~/ios/platform/binding-objective-c/index.md)
