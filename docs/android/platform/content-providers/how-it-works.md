---
title: "콘텐츠 공급자 작업"
ms.topic: article
ms.prod: xamarin
ms.assetid: B9E2EF89-7EBE-45F5-1ED9-7D2C70BE792C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 7802988833563469fcc25e03ee1bda2046591681
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/09/2018
---
# <a name="how-content-providers-work"></a>콘텐츠 공급자 작업

에 관련 된 두 클래스는 `ContentProvider` 상호 작용 합니다.

- **ContentProvider** &ndash; 표준 방식으로 데이터 집합을 노출 하는 API를 구현 합니다. 주요 방법은 쿼리, 삽입, 업데이트 및 삭제 됩니다.

- **ContentResolver** &ndash; 와 통신 하는 정적 프록시는 `ContentProvider` 중 하나에서 동일한 응용 프로그램 내 또는 다른 응용 프로그램에서 해당 데이터에 액세스할 수 있습니다.

콘텐츠 공급자는 일반적으로 SQLite 데이터베이스에서 지원 하지만 API를 사용 하는 코드가 하지 않아도 기본 SQL에 대 한 어떠한 정보도 알 수를 의미 합니다. 쿼리 상수를 사용 하 여 열 이름 (기본 데이터 구조에 대 한 종속성)을 참조 하는 Uri를 통해 완료 되는 및 `ICursor` 반복을 사용 하는 코드에 대해 반환 됩니다.


## <a name="consuming-a-contentprovider"></a>ContentProvider 사용

`ContentProviders` 기능에 등록 되어 있는 Uri 통해는 **AndroidManifest.xml** 데이터를 게시 하는 응용 프로그램입니다. 규칙은 위치 Uri 및 노출 하는 데이터 열에는 해야 쉽게 데이터에 바인딩할 수 있도록 상수로 사용할 수 있습니다. Android의 기본 제공 `ContentProviders` 모든 기능을 제공 편의 클래스의 데이터 구조를 참조 하는 상수는 [ `Android.Providers` ](https://developer.xamarin.com/api/namespace/Android.Provider/) 네임 스페이스입니다.



### <a name="built-in-providers"></a>기본 제공 공급자

Android에서는 다양 한 시스템 및 사용 하 여 사용자 데이터에 대 한 액세스를 제공 `ContentProviders`:

- *브라우저* &ndash; 브라우저 기록 및 책갈피 (권한이 필요 `READ_HISTORY_BOOKMARKS` 및/또는 `WRITE_HISTORY_BOOKMARKS`).

- *콜 로그* &ndash; 최근 호출 하거나 장치에서 수신 합니다.

- *연락처* &ndash; 사용자, 전화, 사진 및 그룹을 포함 하 여 사용자의 연락처 목록에서 정보를 자세히 설명 합니다.

- *MediaStore* &ndash; 사용자의 장치의 내용을: (앨범, 아티스트, 장르, 재생 목록을) 오디오, 이미지 (미리 보기 포함) 및 비디오입니다.

- *설정* &ndash; 시스템 전체 장치 설정 및 기본 설정 합니다.

- *UserDictionary* &ndash; 예측 텍스트 입력에 사용 되는 사용자 지정 사전의 내용을 합니다.

- *음성 메일* &ndash; 음성 메일 메시지의 기록 합니다.



## <a name="classes-overview"></a>클래스 개요

작업할 때 사용 하는 기본 클래스는 `ContentProvider` 여기에 표시 됩니다.

[![콘텐츠 공급자 응용 프로그램 및 응용 프로그램 상호 작용 Consuming 클래스 다이어그램](how-it-works-images/classdiagram1.png)](how-it-works-images/classdiagram1.png#lightbox)

이 다이어그램에는 `ContentProvider` 쿼리를 구현 하 고 URI의 다른 응용 프로그램 데이터를 찾을 사용 하는 등록 합니다. `ContentResolver` 프록시 역할을 하는 ''에 `ContentProvider` (쿼리, 삽입, 업데이트 및 삭제 메서드). `SQLiteOpenHelper` 에서 사용 하는 데이터가 포함 되어는 `ContentProvider`, 있지만 응용 프로그램을 사용 하는 데 직접 노출 되지 않습니다.
`CursorAdapter` 전달 하 여 반환 되는 커서는 `ContentResolver` 에 표시 하는 `ListView`합니다. `UriMatcher` 쿼리를 처리할 때 Uri를 구문 분석 하는 도우미 클래스입니다.

다음은 각 클래스의 용도 대 한 설명입니다.

- **ContentProvider** &ndash; 데이터를 노출 하는이 추상 클래스의이 메서드를 구현 합니다. API가 다른 클래스와 클래스 정의에 추가 하는 Uri 특성을 통해 응용 프로그램에 사용할 수 있습니다.

- **SQLiteOpenHelper** &ndash; 의해 노출 되는 SQLite 데이터 저장소를 구현 하는 데 도움이 됩니다는 `ContentProvider`합니다.

- **UriMatcher** &ndash; 사용 `UriMatcher` 에 프로그램 `ContentProvider` 콘텐츠를 쿼리 하는 데 사용 되는 Uri를 관리할 수 있도록 구현 합니다.

- **ContentResolver** &ndash; 사용 하 여 사용 하는 코드가 `ContentResolver` 액세스로는 `ContentProvider` 인스턴스. 함께 두 클래스는 데이터를 쉽게 응용 프로그램 간에 공유할 수 있도록 프로세스 간 통신 문제를 처리 합니다. 사용 하는 코드가 만들지 않는다는 `ContentProvider` 명시적; 클래스에서 노출 된 Uri에 따라 커서를 만들어서 해당 데이터를 액세스 하는 대신,는 `ContentProvider` 응용 프로그램입니다.

- **CursorAdapter** &ndash; 사용 `CursorAdapter` 또는 `SimpleCursorAdapter` 를 통해 액세스 하는 데이터를 표시 하는 `ContentProvider`합니다.

`ContentProvider` API와 같은 다양 한 데이터에 대 한 작업을 수행할 수 있게 합니다.

-  목록 또는 개별 레코드를 반환 하는 데이터를 쿼리 합니다.
-  개별 레코드를 수정 합니다.
-  새 레코드를 추가 합니다.
-  레코드를 삭제 합니다.

이 문서에서 제공 하 고 시스템을 사용 하는 예제가 포함 되어 `ContentProvider`, 사용자 지정을 구현 하는 간단한 읽기 전용 예제 뿐만 아니라 `ContentProvider`합니다.

