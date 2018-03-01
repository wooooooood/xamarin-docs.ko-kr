---
title: DataPages
ms.topic: article
ms.prod: xamarin
ms.assetid: DF16EAEE-DB78-42CA-9C59-51D9D6CB6B95
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 10c473656947dfea8bf832ae5a5704897ef4ce63
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/28/2018
---
# <a name="datapages"></a>DataPages

![](~/media/shared/preview.png "이 API는 현재 미리 보기")

> [!IMPORTANT]
> DataPages 필요는 [Xamarin.Forms 테마](~/xamarin-forms/user-interface/themes/index.md) 렌더링에 대 한 참조입니다.

Xamarin.Forms DataPages Evolve 2016에서 발표 된 되며 시도 하 고 피드백을 제공 하는 고객에 대 한 미리 보기로 사용할 수 있습니다.

DataPages 쉽고 빠르게 데이터 소스 바인딩할 미리 작성 된 보기에는 API를 제공 합니다. 목록 항목 및 세부 정보 페이지에는 데이터를 자동으로 렌더링 됩니다 및 테마를 사용 하 여 사용자 지정할 수 있습니다.

Evolve 기조연설 데모 작동 하는 방법을 보려면 하려면 체크 아웃 하 고 [시작 가이드](get-started.md)합니다.

[ ![](images/demo-sml.png "DataPages 샘플 응용 프로그램")](images/demo.png "DataPages 샘플 응용 프로그램")

## <a name="introduction"></a>소개

데이터 원본과 관련된 된 데이터 페이지를 쉽고 빠르게 지원 되는 데이터 소스를 사용 하 고 테마를 사용 하는 스 캐 폴딩 하는 UI를 사용자 지정할 수 있는 기본 제공을 사용 하 여 렌더링할 수 있습니다.

DataPages 포함 하 여 Xamarin.Forms 응용 프로그램에 추가 되는 **Xamarin.Forms.Pages** Nuget 패키지 합니다.

### <a name="data-sources"></a>Data Sources

미리 보기에 사용 하기 위해 사용할 수 있는 미리 작성 된 데이터 원본:

* **JsonDataSource**
* **AzureDataSource** (Nuget 구분)
* **AzureEasyTableDataSource** (Nuget 구분)

참조는 [시작 가이드](get-started.md) 사용 하는 예제는 `JsonDataSource`합니다.


### <a name="pages--controls"></a>페이지 및 컨트롤

다음 페이지 및 컨트롤에는 제공 된 데이터 원본에 대 한 간단한 바인딩 수 있도록 포함 되어 있습니다.

* **ListDataPage** – 참조는 [예제 시작](get-started.md)합니다.
* **DirectoryPage** -사용 하도록 설정 하는 그룹화 된 목록입니다.
* **PersonDetailPage** –는 단일 데이터 항목 보기는 특정 개체 유형 (연락처 항목)에 대해 사용자 지정 합니다.
* **DataView** – 일반적인 방법으로 원본에서 데이터를 노출 하려면 보기.
* **CardView** –는 이미지, 제목 텍스트와 설명 텍스트를 포함 하는 뷰에 스타일이 지정 합니다.
* **HeroImage** – 이미지 렌더링 보기.
* **ListItem** 는 – 미리 보기 네이티브 iOS 및 Android 목록 항목에 유사한 레이아웃으로 작성 합니다.

참조는 [DataPages 제어 참조](controls.md) 예입니다.



### <a name="under-the-hood"></a>기본적인 이해

Xamarin.Forms 데이터 소스를 준수 하는 `IDataSource` 인터페이스입니다.

Xamarin.Forms 인프라 다음 속성을 통해 데이터 원본과 상호 작용합니다.

* `Data` -읽기 전용 목록이 표시 될 수 있는 데이터 항목입니다.
* `IsLoading` – 데이터 로드 및 렌더링에 사용할 수 있는지 여부를 나타내는 부울 값입니다.
* `[key]` – 요소를 검색 하기 위한 인덱서를 합니다.

두 가지가 `MaskKey` 및 `UnmaskKey` (표시 하거나 숨길 수) 데이터 항목 속성 (즉, 사용할 수 있는 반영 되지 않도록 하려면 렌더링 대상에서).
키에 해당 하는 데이터 항목 개체에서 명명된 된 속성입니다.

