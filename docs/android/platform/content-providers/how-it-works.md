---
title: 콘텐츠 공급자 작동 방식
ms.prod: xamarin
ms.assetid: B9E2EF89-7EBE-45F5-1ED9-7D2C70BE792C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: e61be6f0189eb825c15fd75764a16706e588ebc9
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73020511"
---
# <a name="how-content-providers-work"></a>콘텐츠 공급자 작동 방식

`ContentProvider` 상호 작용에 관련된 두 가지 클래스가 있습니다.

- **ContentProvider** &ndash; 표준 방식으로 데이터 집합을 노출하는 API를 구현합니다. 주요 메서드는 Query, Insert, Update 및 Delete입니다.

- **ContentResolver** &ndash; 동일한 애플리케이션 내에서 또는 다른 애플리케이션에서 데이터에 액세스하기 위해 `ContentProvider`와 통신하는 정적 프록시입니다.

콘텐츠 공급자는 일반적으로 SQLite 데이터베이스에서 지원되지만 API는 소비 코드가 기본 SQL에 대한 정보를 알 필요가 없음을 의미합니다. 쿼리는 상수를 사용하여 열 이름을 참조하는 Uri를 통해 수행되며(기본 데이터 구조에 대한 종속성을 줄이기 위함) 소비 코드를 반복하기 위해 `ICursor`가 반환됩니다.

## <a name="consuming-a-contentprovider"></a>ContentProvider 사용

`ContentProviders`는 데이터를 게시하는 애플리케이션의 **AndroidManifest.xml**에 등록된 Uri를 통해 해당 기능을 노출합니다. 데이터에 쉽게 바인딩할 수 있도록 노출되는 Uri 및 데이터 열을 상수로 사용해야 하는 규칙이 있습니다. Android의 기본 제공 `ContentProviders`는 모두 상수를 사용하여 [`Android.Providers`](xref:Android.Provider) 네임스페이스의 데이터 구조를 참조하는 편리한 클래스를 제공합니다.

### <a name="built-in-providers"></a>기본 제공 공급자

Android는 `ContentProviders`를 사용하여 다양한 시스템 및 사용자 데이터에 대한 액세스를 제공합니다.

- *Browser* &ndash; 책갈피 및 브라우저 기록(권한 `READ_HISTORY_BOOKMARKS` 및/또는 `WRITE_HISTORY_BOOKMARKS`필요).

- *CallLog* &ndash; 디바이스에서 걸거나 받은 최근 통화.

- *Contacts* &ndash; 사람, 전화, 사진, 그룹 등 사용자의 연락처 목록의 자세한 정보.

- *MediaStore* &ndash; 사용자의 디바이스에 포함된 콘텐츠: 오디오(앨범, 아티스트, 장르, 재생 목록), 이미지(미리 보기 포함) 및 비디오.

- *Settings* &ndash; 시스템 수준 디바이스 설정 및 기본 설정.

- *UserDictionary* &ndash; 예측 텍스트 입력에 사용되는 사용자 정의 사전의 내용.

- *Voicemail* &ndash; 음성 메일 메시지의 기록.

## <a name="classes-overview"></a>클래스 개요

`ContentProvider`에서 작업할 때 사용되는 기본 클래스는 다음과 같습니다.

[![콘텐츠 공급자 애플리케이션 및 소비 애플리케이션 상호 작용의 클래스 다이어그램](how-it-works-images/classdiagram1.png)](how-it-works-images/classdiagram1.png#lightbox)

이 다이어그램에서 `ContentProvider`는 쿼리를 구현하고 다른 애플리케이션에서 데이터를 찾기 위해 사용하는 URI를 등록합니다. `ContentResolver`는 `ContentProvider`(Query, Insert, Update 및 Delete 메서드)에 대한 '프록시' 역할을 합니다. `SQLiteOpenHelper`는 `ContentProvider`에서 사용하는 데이터를 포함하지만 소비 앱에 직접 노출되지 않습니다.
`CursorAdapter`는 `ListView`에 표시하기 위해 `ContentResolver`가 반환하는 커서를 전달합니다. `UriMatcher`는 쿼리를 처리할 때 Uri를 구문 분석하는 도우미 클래스입니다.

각 클래스의 용도는 아래에 설명되어 있습니다.

- **ContentProvider** &ndash; 이 추상 클래스의 메서드를 구현하여 데이터를 노출합니다. API는 클래스 정의에 추가된 Uri 특성을 통해 다른 클래스 및 애플리케이션에서 사용할 수 있습니다.

- **SQLiteOpenHelper** &ndash; `ContentProvider`에서 노출하는 SQLite 데이터 저장소를 구현하는 데 도움이 됩니다.

- **UriMatcher** &ndash; `ContentProvider` 구현에서 `UriMatcher`를 사용하여 콘텐츠를 쿼리하는 데 사용되는 Uri를 관리할 수 있습니다.

- **ContentResolver** &ndash; 소비 코드는 `ContentResolver`를 사용하여 `ContentProvider` 인스턴스에 액세스합니다. 두 클래스를 함께 사용하면 프로세스 간 통신 문제를 처리하여 애플리케이션 사이에 데이터를 쉽게 공유할 수 있습니다. 소비 코드는 `ContentProvider` 클래스를 명시적으로 생성하지 않습니다. 대신 `ContentProvider` 애플리케이션에서 노출하는 Uri에 따라 커서를 만들어 데이터에 액세스합니다.

- **CursorAdapter** &ndash; `CursorAdapter` 또는 `SimpleCursorAdapter`를 사용하여 `ContentProvider`를 통해 액세스되는 데이터를 표시합니다.

`ContentProvider` API를 사용하여 소비자는 다음과 같은 다양한 데이터 작업을 수행할 수 있습니다.

- 데이터를 쿼리하여 목록 또는 개별 레코드를 반환합니다.
- 개별 레코드를 수정합니다.
- 새 레코드를 추가합니다.
- 레코드를 삭제합니다.

이 문서에는 시스템 제공 `ContentProvider`를 사용하는 예제뿐 아니라 사용자 지정 `ContentProvider`를 구현하는 간단한 읽기 전용 예제도 포함되어 있습니다.
