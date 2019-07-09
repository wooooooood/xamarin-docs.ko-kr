---
title: Xamarin.Forms 로컬 데이터 저장소
description: 파일 공유 Xamarin.Forms 코드에서 처리를 수행 하는 방법 및 SQLite.Net를 사용 하 여 로컬 SQLite 데이터베이스에 데이터를 읽고 하는 방법에 알아봅니다.
ms.prod: xamarin
ms.assetid: A324C247-7DA8-4B14-A813-25F85525E32B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2019
ms.openlocfilehash: 585b55b79046af07e466fb25b33f4c22c2ec5e5b
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67659070"
---
# <a name="xamarinforms-local-data-storage"></a>Xamarin.Forms 로컬 데이터 저장소

## <a name="filesfilesmd"></a>[파일](files.md)

Xamarin.Forms를 통한 파일 처리는 .NET Standard 라이브러리의 코드를 사용하거나 포함 리소스를 사용하여 수행할 수 있습니다. 이 문서에서는 Xamarin.Forms 응용 프로그램에서 공유 코드에서 처리 하는 파일을 수행 하는 방법을 설명 합니다.

## <a name="local-databasesdatabasesmd"></a>[로컬 데이터베이스](databases.md)

Xamarin.Forms는 SQLite 데이터베이스 엔진을 사용하여 데이터베이스 기반 애플리케이션을 지원하기 때문에 공유 코드로 개체를 로드하고 저장할 수 있습니다. 이 문서에서는 Xamarin.Forms 애플리케이션이 SQLite.Net을 사용하여 데이터를 읽고 로컬 SQLite 데이터베이스에 데이터를 기록하는 방법을 설명합니다.
