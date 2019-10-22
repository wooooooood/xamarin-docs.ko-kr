---
title: Xamarin Forms 웹 서비스 소개
description: 이 가이드에서는 다양 한 웹 서비스와 통신 하는 방법을 보여 주는 Xamarin.ios 예제 응용 프로그램의 연습을 제공 합니다. 각 웹 서비스는 별도의 샘플 응용 프로그램을 사용 하지만, 기능적으로 비슷하며 공통 클래스를 공유 합니다.
ms.prod: xamarin
ms.assetid: A3FEB262-0D79-42E6-8F8B-A565618C490B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: bbeab6a6ab0d4a9d0e3a962240317fc0d54f9e25
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "68656646"
---
# <a name="xamarinforms-web-services-introduction"></a>Xamarin Forms 웹 서비스 소개

[![샘플 다운로드](~/media/shared/download.png) 샘플 다운로드](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todorest)

_이 항목에서는 다양 한 웹 서비스와 통신 하는 방법을 보여 주는 Xamarin.ios 예제 응용 프로그램의 연습을 제공 합니다. 각 웹 서비스는 별도의 샘플 응용 프로그램을 사용 하지만, 기능적으로 비슷하며 공통 클래스를 공유 합니다._

아래에 설명 된 샘플 할 일 목록 응용 프로그램은 Xamarin.ios를 사용 하 여 다양 한 유형의 웹 서비스 백 엔드에 액세스 하는 방법을 보여 주는 데 사용 됩니다. 다음에 대 한 기능을 제공 합니다.

- 작업 목록을 봅니다.
- 작업을 추가, 편집 및 삭제 합니다.
- 작업 상태를 ' 완료 '로 설정 합니다.
- 작업의 이름 및 메모 필드를 말합니다.

모든 경우에 작업은 웹 서비스를 통해 액세스 되는 백 엔드에 저장 됩니다.

응용 프로그램이 시작 되 면 웹 서비스에서 검색 된 모든 작업을 나열 하는 페이지가 표시 되 고 사용자가 새 작업을 만들 수 있습니다. 작업을 클릭 하면 응용 프로그램에서 작업을 편집, 저장, 삭제 및 읽을 수 있는 두 번째 페이지로 이동 합니다. 최종 애플리케이션은 다음과 같습니다.

![](introduction-images/app-example-1.png "Todo application - first page")
![](introduction-images/app-example-2.png "Todo application - second page")

이 가이드의 각 항목에서는 특정 유형의 웹 서비스 백 엔드를 설명 하는 *다른* 버전의 응용 프로그램에 대 한 다운로드 링크를 제공 합니다. 각 웹 서비스 스타일과 관련 된 페이지에서 관련 샘플 코드를 다운로드 합니다.

## <a name="understand-the-application-anatomy"></a>응용 프로그램 분석 이해

각 샘플 응용 프로그램에 대 한 공유 코드 프로젝트는 다음과 같은 세 가지 주요 폴더로 구성 됩니다.

|폴더|용도|
|--- |--- |
|데이터|데이터 항목을 관리 하 고 웹 서비스와 통신 하는 데 사용 되는 클래스 및 인터페이스를 포함 합니다. 여기에는 최소한 웹 서비스 작업을 호출 하기 위해 `App` 클래스의 속성을 통해 노출 되는 `TodoItemManager` 클래스가 포함 됩니다.|
|모델|응용 프로그램에 대 한 데이터 모델 클래스를 포함 합니다. 여기에는 최소한 응용 프로그램에서 사용 하는 데이터의 단일 항목을 모델링 하는 `TodoItem` 클래스가 포함 됩니다. 이 폴더에는 사용자 데이터를 모델링 하는 데 사용 되는 추가 클래스도 포함 될 수 있습니다.|
|보기|응용 프로그램에 대 한 페이지를 포함 합니다. 이는 일반적으로 `TodoListPage` 및 `TodoItemPage` 클래스와 인증 목적으로 사용 되는 추가 클래스로 구성 됩니다.|

또한 각 응용 프로그램에 대 한 공유 코드 프로젝트는 여러 중요 한 파일로 구성 되어 있습니다.

|파일|용도|
|--- |--- |
|Constants.cs|응용 프로그램에서 웹 서비스와 통신 하는 데 사용 하는 상수를 지정 하는 `Constants` 클래스입니다. 이러한 상수는 공급자에서 만든 개인 백엔드 서비스에 액세스 하기 위해 업데이트 해야 합니다.|
|ITextToSpeech.cs|@No__t_0 인터페이스는 구현 클래스에서 `Speak` 메서드를 제공 하도록 지정 합니다.|
|Todo.cs|각 플랫폼에서 응용 프로그램에 의해 표시 되는 첫 페이지와 웹 서비스 작업을 호출 하는 데 사용 되는 `TodoItemManager` 클래스의 인스턴스화를 담당 하는 `App` 클래스입니다.|

### <a name="view-pages"></a>페이지 보기

대부분의 샘플 응용 프로그램에는 두 개 이상의 페이지가 포함 되어 있습니다.

- **TodoListPage** –이 페이지에는 `TodoItem` 인스턴스 목록과 `TodoItem.Done` 속성이 `true` 경우 눈금 아이콘이 표시 됩니다. 항목을 클릭 하면 `TodoItemPage` 이동 합니다. 또한 *+* 기호를 클릭 하 여 새 항목을 만들 수 있습니다.
- **TodoItemPage** –이 페이지에서는 선택 된 `TodoItem`에 대 한 세부 정보를 표시 하 고 편집, 저장, 삭제 및 음성으로 지정할 수 있습니다.

또한 일부 샘플 응용 프로그램에는 사용자 인증 프로세스를 관리 하는 데 사용 되는 추가 페이지가 포함 되어 있습니다.

### <a name="model-the-data"></a>데이터 모델링

각 샘플 응용 프로그램은 `TodoItem` 클래스를 사용 하 여 표시 되 고 저장을 위해 웹 서비스로 전송 되는 데이터를 모델링 합니다. 다음 코드 예제는 `TodoItem` 클래스를 보여줍니다.

```csharp
public class TodoItem
{
    public string ID { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
}
```

@No__t_0 속성은 각 `TodoItem` 인스턴스를 고유 하 게 식별 하는 데 사용 되며 각 웹 서비스에서 업데이트 하거나 삭제할 데이터를 식별 하는 데 사용 됩니다.

### <a name="invoke-web-service-operations"></a>웹 서비스 작업 호출

웹 서비스 작업은 `TodoItemManager` 클래스를 통해 액세스 되 고 클래스의 인스턴스는 `App.TodoManager` 속성을 통해 액세스할 수 있습니다. @No__t_0 클래스는 웹 서비스 작업을 호출 하는 다음과 같은 메서드를 제공 합니다.

- **Getthe async** -이 메서드는 `TodoListPage`의 `ListView` 컨트롤을 웹 서비스에서 검색 된 `TodoItem` 인스턴스로 채우는 데 사용 됩니다.
- **Savetaskasync** –이 메서드는 웹 서비스의 `TodoItem` 인스턴스를 만들거나 업데이트 하는 데 사용 됩니다.
- **Deletetaskasync** –이 메서드는 웹 서비스의 `TodoItem` 인스턴스를 삭제 하는 데 사용 됩니다.

또한 일부 샘플 응용 프로그램에는 사용자 인증 프로세스를 관리 하는 데 사용 되는 `TodoItemManager` 클래스의 추가 메서드가 포함 되어 있습니다.

@No__t_0 메서드는 웹 서비스 작업을 직접 호출 하는 대신 `TodoItemManager` 생성자에 삽입 되는 종속 클래스에서 메서드를 호출 합니다. 예를 들어 한 샘플 응용 프로그램은 `TodoItemManager` 생성자에 `RestService` 클래스를 삽입 하 여 데이터에 액세스 하는 데 REST Api를 사용 하는 구현을 제공 합니다.

## <a name="related-links"></a>관련 링크

- [ASMX (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todoasmx)
- [WCF (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todowcf)
- [REST (샘플)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todorest)
