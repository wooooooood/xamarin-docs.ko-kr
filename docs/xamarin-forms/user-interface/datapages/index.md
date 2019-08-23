---
title: Xamarin. 양식 DataPages
description: 이 문서에서는 미리 작성 된 뷰에 데이터 원본을 빠르고 쉽게 바인딩하는 API를 제공 하는 Xamarin.ios DataPages를 소개 합니다.
ms.prod: xamarin
ms.assetid: DF16EAEE-DB78-42CA-9C59-51D9D6CB6B95
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 9dc34f412549c4be6099b373ddae7cbd2e8d21c8
ms.sourcegitcommit: 41a029c69925e3a9d2de883751ebfd649e8747cd
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68980780"
---
# <a name="xamarinforms-datapages"></a>Xamarin. 양식 DataPages

![](~/media/shared/preview.png "이 API는 현재 미리 보기")

> [!IMPORTANT]
> DataPages를 렌더링 하려면 Xamarin.ios 테마 참조가 필요 합니다. 이 것은 프로젝트에 [Xamarin.Forms.Theme.Base](https://www.nuget.org/packages/Xamarin.Forms.Theme.Base/) NuGet 패키지 설치를 유발하고, [Xamarin.Forms.Theme.Light](https://www.nuget.org/packages/Xamarin.Forms.Theme.Light/) 및 [Xamarin.Forms.Theme.Dark](https://www.nuget.org/packages/Xamarin.Forms.Theme.Dark/) NuGet 패키지 전체에로 따른다.

Xamarin. Forms DataPages는 2016에 발표 되었으며 고객이 피드백을 제공 하 고 피드백을 제공 하는 미리 보기로 제공 됩니다.

DataPages 빠르고 쉽게 미리 작성된 된 보기에 데이터 소스를 바인딩하는 API를 제공 합니다. 목록 항목 및 세부 정보 페이지는 데이터를 자동으로 렌더링 하며 테마를 사용 하 여 사용자 지정할 수 있습니다.

진화 키 노트 데모의 작동 방식을 확인 하려면 [시작 가이드](get-started.md)를 확인 하세요.

[![](images/demo-sml.png "DataPages 샘플 응용 프로그램")](images/demo.png#lightbox "DataPages 샘플 응용 프로그램")

## <a name="introduction"></a>소개

개발자는 데이터 원본 및 연결 된 데이터 페이지를 사용 하 여 지원 되는 데이터 원본을 빠르고 쉽게 사용 하 고 테마를 사용 하 여 사용자 지정할 수 있는 기본 제공 UI 스 캐 폴딩을 사용 하 여 렌더링할 수 있습니다.

DataPages는 xamarin.ios Nuget 패키지를 포함 하 여 Xamarin.ios 응용 프로그램 에 추가 됩니다.

### <a name="data-sources"></a>Data Sources

미리 보기에는 몇 가지 미리 작성 된 데이터 원본을 사용할 수 있습니다.

* **JsonDataSource**
* **Azuredatasource** (별도의 Nuget)
* **AzureEasyTableDataSource** (별도의 Nuget)

을`JsonDataSource`사용 하는 예제는 [시작 가이드](get-started.md) 를 참조 하세요.


### <a name="pages--controls"></a>페이지 & 컨트롤

제공 된 데이터 원본에 쉽게 바인딩할 수 있도록 다음 페이지 및 컨트롤이 포함 되어 있습니다.

* **Listdatapage** – [시작 예제](get-started.md)를 참조 하세요.
* **DirectoryPage** – 그룹화가 설정 된 목록입니다.
* **PersonDetailPage** – 특정 개체 유형 (연락처 항목)에 대해 사용자 지정 된 단일 데이터 항목 뷰입니다.
* **DataView** – 제네릭 방식으로 원본의 데이터를 노출 하는 뷰입니다.
* **CardView** – 이미지, 제목 텍스트 및 설명 텍스트를 포함 하는 스타일 있는 뷰입니다.
* **HeroImage** – 이미지 렌더링 뷰입니다.
* **ListItem** – 기본 IOS 및 Android 목록 항목과 비슷한 레이아웃이 포함 된 미리 작성 된 뷰입니다.

예제는 [Datapages 컨트롤 참조](controls.md) 를 참조 하세요.



### <a name="under-the-hood"></a>내부적으로

Xamarin Forms 데이터 소스는 `IDataSource` 인터페이스를 따릅니다.

Xamarin.ios 인프라는 다음 속성을 통해 데이터 원본과 상호 작용 합니다.

* `Data`– 표시할 수 있는 데이터 항목의 읽기 전용 목록입니다.
* `IsLoading`– 데이터를 로드 하 고 렌더링할 수 있는지 여부를 나타내는 부울입니다.
* `[key]`– 요소를 검색 하는 인덱서입니다.

데이터 항목 속성 ( `MaskKey` 또는 `UnmaskKey` 표시)을 숨기 거 나 표시 하는 데 사용할 수 있는 두 가지 방법이 있습니다. 렌더링 되지 않도록 합니다).
키는 데이터 항목 개체의 명명 된 속성에 해당 합니다.
