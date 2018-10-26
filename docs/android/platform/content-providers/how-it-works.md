---
title: 콘텐츠 공급자 작업
ms.prod: xamarin
ms.assetid: B9E2EF89-7EBE-45F5-1ED9-7D2C70BE792C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: df4c2e10e34c308e4fadb44fba9c6a14714ae1b9
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108940"
---
# <a name="how-content-providers-work"></a>콘텐츠 공급자 작업

관련 된 두 클래스는 `ContentProvider` 상호 작용 합니다.

- **ContentProvider** &ndash; 표준 방식으로 데이터 집합을 노출 하는 API를 구현 합니다. 주요 방법은 쿼리, 삽입, 업데이트 및 삭제 됩니다.

- **ContentResolver** &ndash; 통신 하는 정적 프록시는 `ContentProvider` 또는 동일한 응용 프로그램 내에서 다른 응용 프로그램에서 해당 데이터에 액세스할 수 있습니다.

콘텐츠 공급자는 일반적으로 SQLite 데이터베이스에서 지원 하지만 API는 코드를 사용 하지 않아도 기본 SQL에 대 한 지식이 의미 합니다. 열 이름 (기본 데이터 구조에 대 한 종속성)를 참조 하려면 상수를 사용 하 여 쿼리 Uri를 통해 수행 됩니다 및 `ICursor` 반복을 사용 하는 코드에 대해 반환 됩니다.


## <a name="consuming-a-contentprovider"></a>ContentProvider 사용

`ContentProviders` 에 등록 된 Uri 통해 해당 기능을 노출 합니다 **AndroidManifest.xml** 데이터를 게시 하는 응용 프로그램입니다. 규칙은 위치 Uri 및 노출 되는 데이터 열에는 사용 가능한 것으로 데이터에 바인딩할 수 있도록 하는 상수입니다. Android의 기본 제공 `ContentProviders` 모두의 데이터 구조를 참조 하는 상수를 사용 하 여 편리 하 게 클래스를 제공 합니다 [ `Android.Providers` ](https://developer.xamarin.com/api/namespace/Android.Provider/) 네임 스페이스입니다.



### <a name="built-in-providers"></a>기본 제공 공급자

Android는 광범위 한 시스템 및 사용 하 여 사용자 데이터에 대 한 액세스를 제공 `ContentProviders`:

- *브라우저* &ndash; 책갈피 및 브라우저 기록 (권한이 필요 `READ_HISTORY_BOOKMARKS` 및/또는 `WRITE_HISTORY_BOOKMARKS`).

- *콜 로그* &ndash; 최근 호출 수행 하거나 장치를 사용 하 여 수신 합니다.

- *연락처* &ndash; 사람들, 전화, 사진 및 그룹을 포함 하 여 사용자의 연락처 목록에서 정보를 자세히 설명 합니다.

- *MediaStore* &ndash; 내용의 사용자의 장치: (앨범에서 아티스트, 장르, 재생) 오디오, 이미지 (미리 보기 포함) 및 비디오입니다.

- *설정을* &ndash; 시스템 장치 설정 및 기본 설정 합니다.

- *UserDictionary* &ndash; 내용의 예측 텍스트 입력에 사용 되는 사용자 정의 사전입니다.

- *음성* &ndash; 음성 메일 메시지를 기록 합니다.



## <a name="classes-overview"></a>클래스 개요

작업할 때 사용 하는 기본 클래스는 `ContentProvider` 여기에 표시 됩니다.

[![콘텐츠 공급자 응용 프로그램 및 응용 프로그램 상호 작용 사용의 클래스 다이어그램](how-it-works-images/classdiagram1.png)](how-it-works-images/classdiagram1.png#lightbox)

이 다이어그램에는 `ContentProvider` 쿼리를 구현 하 고 등록 URI의 다른 응용 프로그램 데이터를 찾는 데 사용 합니다. 합니다 `ContentResolver` 프록시로 ''에 `ContentProvider` (쿼리, 삽입, 업데이트 및 삭제 메서드). `SQLiteOpenHelper` 사용 되는 데이터를 포함 합니다 `ContentProvider`, 있지만 앱을 사용 하 여 직접 노출 되지 않습니다.
합니다 `CursorAdapter` 반환 되는 커서를 전달 합니다 `ContentResolver` 표시할를 `ListView`입니다. `UriMatcher` 쿼리를 처리 하는 경우에 Uri를 구문 분석 하는 도우미 클래스입니다.

각 클래스의 용도 대 한 설명은 다음과 같습니다.

- **ContentProvider** &ndash; 데이터를 노출 하려면이 추상 클래스의이 메서드를 구현 합니다. API는 다른 클래스와 클래스 정의에 추가 되는 Uri 특성을 통해 응용 프로그램에 제공 됩니다.

- **SQLiteOpenHelper** &ndash; 사용 하면 구현에 의해 노출 되는 SQLite 데이터 저장소는 `ContentProvider`합니다.

- **UriMatcher** &ndash; 사용 하 여 `UriMatcher` 에서 프로그램 `ContentProvider` 콘텐츠를 쿼리 하는 데 사용 되는 Uri를 관리할 수 있도록 구현 합니다.

- **ContentResolver** &ndash; 사용 하 여 코드를 사용을 `ContentResolver` 액세스 하는 `ContentProvider` 인스턴스. 함께 두 클래스의 데이터를 쉽게 응용 프로그램 간에 공유할 수 있도록 프로세스 간 통신 문제에 주의 합니다. 만들지 않는다는 코드가 사용을 `ContentProvider` 명시적; 클래스에서 노출 된 Uri에 따라 커서를 만들어 해당 데이터를 액세스 하는 대신는 `ContentProvider` 응용 프로그램입니다.

- **CursorAdapter** &ndash; 사용 하 여 `CursorAdapter` 하거나 `SimpleCursorAdapter` 를 통해 액세스 하는 데이터를 표시 하는 `ContentProvider`합니다.

`ContentProvider` API와 같은 다양 한 데이터에 대 한 작업을 수행할 수 있게 합니다.

-  목록 또는 개별 레코드를 반환 하는 데이터를 쿼리 합니다.
-  개별 레코드를 수정 합니다.
-  새 레코드를 추가 합니다.
-  레코드를 삭제 합니다.

이 문서에서 제공 하 고 시스템을 사용 하는 예가 포함 되어 있습니다 `ContentProvider`, 사용자 지정을 구현 하는 간단한 읽기 전용 예제 뿐만 아니라 `ContentProvider`합니다.

