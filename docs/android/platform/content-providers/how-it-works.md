---
title: 콘텐츠 공급자의 작동 방식
ms.prod: xamarin
ms.assetid: B9E2EF89-7EBE-45F5-1ED9-7D2C70BE792C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: e61be6f0189eb825c15fd75764a16706e588ebc9
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020511"
---
# <a name="how-content-providers-work"></a>콘텐츠 공급자의 작동 방식

`ContentProvider` 상호 작용에 관련 된 두 가지 클래스가 있습니다.

- **Contentprovider** &ndash;는 표준 방식으로 데이터 집합을 노출 하는 API를 구현 합니다. 주요 메서드는 쿼리, 삽입, 업데이트 및 삭제입니다.

- **Contentresolver** &ndash; 동일한 응용 프로그램이 나 다른 응용 프로그램 내에서 데이터에 액세스 하기 위해 `ContentProvider`와 통신 하는 정적 프록시입니다.

콘텐츠 공급자는 일반적으로 SQLite 데이터베이스에서 지원 되지만 API는 코드를 사용 하 여 기본 SQL에 대 한 정보를 알 필요가 없음을 의미 합니다. 쿼리는 상수를 사용 하 여 열 이름을 참조 하는 Uri를 통해 수행 되며 (기본 데이터 구조에 대 한 종속성을 줄이기 위해) 사용 하는 코드를 반복 하기 위해 `ICursor` 반환 됩니다.

## <a name="consuming-a-contentprovider"></a>ContentProvider 사용

`ContentProviders`는 데이터를 게시 하는 응용 프로그램의 .xml에 등록 된 Uri를 통해 해당 기능을 노출 합니다 **.** 노출 되는 Uri 및 데이터 열을 상수로 사용 하 여 데이터에 쉽게 바인딩할 수 있는 규칙이 있습니다. Android의 기본 제공 `ContentProviders`는 모두 [`Android.Providers`](xref:Android.Provider) 네임 스페이스의 데이터 구조를 참조 하는 상수를 사용 하 여 편리한 클래스를 제공 합니다.

### <a name="built-in-providers"></a>기본 제공 공급자

Android는 `ContentProviders`을 사용 하 여 다양 한 시스템 및 사용자 데이터에 대 한 액세스를 제공 합니다.

- *브라우저* &ndash; 책갈피 및 브라우저 기록 (권한 `READ_HISTORY_BOOKMARKS` 및/또는 `WRITE_HISTORY_BOOKMARKS`필요).

- *Calllog* 는 장치를 사용 하 여 최근 전화를 걸거나 수신 &ndash;.

- *연락처* 는 사람, 휴대폰, 사진 & 그룹을 포함 하 여 사용자의 연락처 목록에서 자세한 정보를 &ndash; 합니다.

- *Mediastore* 는 사용자 장치의 오디오 (앨범, 아티스트, 장르, 재생 목록), 이미지 (미리 보기 포함) & 비디오 &ndash; 콘텐츠를 포함 합니다.

- *설정* &ndash; 시스템 차원의 장치 설정 및 기본 설정입니다.

- *Userdictionary* &ndash; 예측 텍스트 입력에 사용 되는 사용자 정의 사전의 내용입니다.

- *음성 메일 &ndash; 음성* 메일 메시지의 기록입니다.

## <a name="classes-overview"></a>클래스 개요

`ContentProvider` 작업할 때 사용 되는 기본 클래스는 다음과 같습니다.

[콘텐츠 공급자 응용 프로그램의![클래스 다이어그램 및 응용 프로그램 상호 작용 사용](how-it-works-images/classdiagram1.png)](how-it-works-images/classdiagram1.png#lightbox)

이 다이어그램에서 `ContentProvider`는 쿼리를 구현 하 고 다른 응용 프로그램에서 데이터를 찾기 위해 사용 하는 URI를 등록 합니다. `ContentResolver` `ContentProvider` (쿼리, 삽입, 업데이트 및 삭제 메서드)에 대 한 ' 프록시 ' 역할을 합니다. `SQLiteOpenHelper`는 `ContentProvider`에서 사용 하는 데이터를 포함 하지만 앱을 소비 하는 데 직접 노출 되지 않습니다.
`CursorAdapter`는 `ListView`에 표시 하기 위해 `ContentResolver`에서 반환 된 커서를 전달 합니다. `UriMatcher`는 쿼리를 처리할 때 Uri를 구문 분석 하는 도우미 클래스입니다.

각 클래스의 용도는 아래에 설명 되어 있습니다.

- **Contentprovider** &ndash;이 추상 클래스의 메서드를 구현 하 여 데이터를 노출 합니다. API는 클래스 정의에 추가 된 Uri 특성을 통해 다른 클래스 및 응용 프로그램에서 사용할 수 있게 됩니다.

- **SQLiteOpenHelper** &ndash; `ContentProvider`에서 노출 하는 SQLite 데이터 저장소를 구현 하는 데 도움이 됩니다.

- **UriMatcher** &ndash; `ContentProvider` 구현에서 `UriMatcher`를 사용 하 여 콘텐츠를 쿼리 하는 데 사용 되는 uri를 관리할 수 있도록 합니다.

- **Contentresolver** &ndash; 사용 하는 코드는 `ContentResolver`를 사용 하 여 `ContentProvider` 인스턴스에 액세스 합니다. 두 클래스를 함께 사용 하면 프로세스 간 통신 문제를 처리 하 여 응용 프로그램 간에 데이터를 쉽게 공유할 수 있습니다. 코드를 사용 하면 `ContentProvider` 클래스 명시적으로 생성 되지 않습니다. 대신 `ContentProvider` 응용 프로그램에서 노출 하는 Uri에 따라 커서를 만들어 데이터에 액세스 합니다.

- **CursorAdapter** &ndash; `CursorAdapter` 또는 `SimpleCursorAdapter`를 사용 하 여 `ContentProvider`를 통해 액세스 되는 데이터를 표시 합니다.

`ContentProvider` API를 사용 하 여 소비자는 다음과 같은 다양 한 데이터 작업을 수행할 수 있습니다.

- 데이터를 쿼리하여 목록 또는 개별 레코드를 반환 합니다.
- 개별 레코드를 수정 합니다.
- 새 레코드를 추가 합니다.
- 레코드를 삭제 합니다.

이 문서에는 시스템에서 제공 하는 `ContentProvider`를 사용 하는 예제와 사용자 지정 `ContentProvider`를 구현 하는 간단한 읽기 전용 예가 포함 되어 있습니다.
