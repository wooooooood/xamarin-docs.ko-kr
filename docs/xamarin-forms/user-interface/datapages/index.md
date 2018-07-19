---
title: Xamarin.Forms DataPages
description: 이 문서에서는 신속 하 게 하기 위한 API를 제공 하 고 쉽게 미리 작성된 된 보기에 데이터 소스를 바인딩하는 Xamarin.Forms DataPages를 소개 합니다.
ms.prod: xamarin
ms.assetid: DF16EAEE-DB78-42CA-9C59-51D9D6CB6B95
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 2a74b636a41a72b26776157a774f0a33ef45a075
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38815889"
---
# <a name="xamarinforms-datapages"></a>Xamarin.Forms DataPages

![](~/media/shared/preview.png "이 API는 현재 미리 보기")

> [!IMPORTANT]
> DataPages 필요는 [Xamarin.Forms 테마](~/xamarin-forms/user-interface/themes/index.md) 렌더링에 대 한 참조입니다.

Xamarin.Forms DataPages 진화 2016에서 발표 된 및을 사용해 보고 피드백을 제공 하는 고객에 대 한 미리 보기로 제공 됩니다.

DataPages 빠르고 쉽게 미리 작성된 된 보기에 데이터 소스를 바인딩하는 API를 제공 합니다. 목록 항목 및 세부 정보 페이지에 데이터를 자동으로 렌더링 됩니다 및 테마를 사용 하 여 사용자 지정할 수 있습니다.

어떻게 진화 기조 연설 데모 작동 하는지 확인 합니다 [시작 가이드](get-started.md)합니다.

[![](images/demo-sml.png "DataPages 샘플 응용 프로그램")](images/demo.png#lightbox "DataPages 샘플 응용 프로그램")

## <a name="introduction"></a>소개

데이터 원본 및 연결 된 데이터 페이지 개발자가 쉽고 신속 하 게 지원 되는 데이터 원본을 사용 하 고 테마를 사용 하 여 기본 제공 하는 스 캐 폴딩 하는 UI를 사용자 지정할 수 있습니다를 사용 하 여 렌더링을 허용 합니다.

DataPages 포함 하 여 Xamarin.Forms 응용 프로그램에 추가 되는 **Xamarin.Forms.Pages** Nuget 패키지.

### <a name="data-sources"></a>Data Sources

미리 보기에 일부 미리 작성 된 데이터 소스를 사용할 수 있습니다.

* **JsonDataSource**
* **AzureDataSource** (별도 Nuget)
* **AzureEasyTableDataSource** (별도 Nuget)

참조를 [시작 가이드](get-started.md) 사용 하는 예는 `JsonDataSource`합니다.


### <a name="pages--controls"></a>페이지 및 컨트롤

다음 페이지 및 컨트롤을 제공 된 데이터 원본에 쉽게 바인딩할 수 있도록 포함 되어 있습니다.

* **ListDataPage** – 참조 합니다 [예제에서는 시작](get-started.md)합니다.
* **DirectoryPage** – 사용 하도록 설정 하는 그룹화 된 목록입니다.
* **PersonDetailPage** – 단일 데이터 항목을 특정 개체 형식 (연락처 항목)에 대 한 사용자 지정 보기.
* **DataView** –는 일반적으로 원본에서 데이터를 노출 하는 보기입니다.
* **CardView** – 보기는 이미지, 제목 텍스트 및 설명 텍스트를 포함 하는 스타일입니다.
* **HeroImage** – 이미지 렌더링 보기를 합니다.
* **ListItem** – 미리 작성 된 네이티브 iOS 및 Android 목록 항목와 비슷한 레이아웃을 사용 하 여 보기.

참조 된 [DataPages 제어 참조](controls.md) 예입니다.



### <a name="under-the-hood"></a>내부 살펴보기

Xamarin.Forms 데이터 소스를 준수 하는 `IDataSource` 인터페이스입니다.

Xamarin.Forms 인프라 다음 속성을 통해 데이터 원본과 상호 작용 합니다.

* `Data` – 읽기 전용으로 표시 될 수 있는 데이터 항목 목록이 있습니다.
* `IsLoading` -데이터 로드 및 렌더링에 사용할 수 있는지 여부를 나타내는 부울입니다.
* `[key]` – 요소를 검색 하기 위한 인덱서를 합니다.

두 가지가 있습니다 `MaskKey` 고 `UnmaskKey` (즉 데이터 항목 속성을 숨기기 (또는 표시)를 사용할 수 있는 않도록 렌더링 대상에서).
키에 해당 하는 데이터 항목 개체에서 명명된 된 속성입니다.
