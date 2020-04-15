---
title: ContentProviders 소개
description: Android 운영 체제는 콘텐츠 공급자를 사용하여 미디어 파일, 연락처, 일정 정보와 같은 공유 데이터에 쉽게 액세스할 수 있습니다. 이 문서에서는 ContentProvider 클래스를 소개하고 이를 사용하는 방법에 대한 두 가지 예제를 제공합니다.
ms.prod: xamarin
ms.assetid: 6E1810AA-EB70-9AD0-1B32-D9418908CC97
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 496e5c092c79f4f71bddaad30bea6acd1d58d375
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027550"
---
# <a name="intro-to-contentproviders"></a>ContentProviders 소개

_Android 운영 체제는 콘텐츠 공급자를 사용하여 미디어 파일, 연락처, 일정 정보와 같은 공유 데이터에 쉽게 액세스할 수 있습니다. 이 문서에서는 ContentProvider 클래스를 소개하고 이를 사용하는 방법에 대한 두 가지 예제를 제공합니다._

## <a name="content-providers-overview"></a>콘텐츠 공급자 개요

*ContentProvider*는 데이터 리포지토리를 캡슐화하고 이에 액세스할 수 있는 API를 제공합니다. 공급자는 일반적으로 데이터를 표시/관리하기 위한 UI도 제공하는 Android 애플리케이션의 일부로 존재합니다. 콘텐츠 공급자를 사용할 경우의 이점은 다른 애플리케이션이 *ContentResolver*라고 하는 공급자 클라이언트 개체를 사용하여 캡슐화된 데이터에 쉽게 액세스할 수 있다는 것입니다. 콘텐츠 공급자와 콘텐츠 확인자는 쉽게 빌드하고 사용할 수 있는 일관된 애플리케이션 간 API(데이터 액세스용)를 제공합니다. 모든 애플리케이션은 `ContentProviders`를 사용하여 내부적으로 데이터를 관리하며, 다른 애플리케이션에 데이터를 노출하도록 선택할 수 있습니다.

애플리케이션에서 사용자 지정 검색 제안을 제공하거나 애플리케이션에서 복잡한 데이터를 복사하여 다른 애플리케이션에 붙여넣는 기능을 제공하려는 경우에도 `ContentProvider`가 필요합니다. 이 문서에서는 Xamarin.Android를 사용하여 `ContentProviders`를 액세스하고 빌드하는 방법을 보여줍니다.

이 섹션은 다음과 같이 구성됩니다.

- **작동 방식** &ndash; `ContentProvider`의 디자인 의도 및 작동 방식의 개요.

- **콘텐츠 공급자 사용** &ndash; 연락처 목록에 액세스하는 예제.

- **ContentProvider를 사용하여 데이터 공유** &ndash; 동일한 애플리케이션에서 `ContentProvider` 작성 및 사용.

`ContentProviders`와 해당 데이터에 대해 작동하는 커서는 Listview를 채우는 데 주로 사용됩니다. 이러한 클래스를 사용하는 방법에 대한 자세한 내용은 [Listview 및 어댑터 가이드](~/android/user-interface/layouts/list-view/index.md)를 참조하세요.

Android 또는 기타 애플리케이션에 의해 노출되는 `ContentProviders`는 애플리케이션의 다른 원본으로부터 데이터를 포함하는 간단한 방법입니다. 이를 통해 애플리케이션 내에서 연락처 목록, 사진 또는 일정 이벤트와 같은 데이터에 액세스하고 표시할 수 있으며, 사용자가 해당 데이터와 상호 작용할 수 있습니다.

사용자 지정 `ContentProviders`는 자신의 앱 내에서 사용하거나 다른 애플리케이션에서 사용할 수 있도록 데이터를 패키지하는 편리한 방법입니다(사용자 지정 검색 및 복사/붙여넣기와 같은 특수한 사용 포함).

이 섹션의 항목에서는 `ContentProvider` 코드를 사용하고 작성하는 몇 가지 간단한 예제를 제공합니다.

## <a name="related-links"></a>관련 링크

- [ContactsAdapter 데모(샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-contactsadapterdemo)
- [SimpleContentProvider(샘플)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/platformfeatures-simplecontentprovider)
- [콘텐츠 공급자 개발자 가이드](https://developer.android.com/guide/topics/providers/content-providers.html)
- [ContentProvider 클래스 참조](xref:Android.Content.ContentProvider)
- [ContentResolver 클래스 참조](xref:Android.Content.ContentResolver)
- [ListView 클래스 참조](xref:Android.Widget.ListView)
- [CursorAdapter 클래스 참조](xref:Android.Widget.CursorAdapter)
- [UriMatcher 클래스 참조](xref:Android.Content.UriMatcher)
- [Android.Provider](xref:Android.Provider)
- [ContactsContract 클래스 참조](xref:Android.Provider.ContactsContract)
