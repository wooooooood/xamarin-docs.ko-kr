---
title: Xamarin.Forms 지역화
description: 기본 제공되는 .NET 지역화 프레임워크는 Xamarin.Forms로 플랫폼 간 다국어 애플리케이션을 빌드하는 데 사용할 수 있습니다. 텍스트 및 이미지를 지역화할 수 있고 애플리케이션은 오른쪽에서 왼쪽 흐름 방향을 지원할 수 있습니다.
ms.prod: xamarin
ms.assetid: 97BF843B-BDAA-4CEA-8189-6DB54B291D7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: dddb80e8fb683547d2bf6ba0b74e870fe240695c
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137568"
---
# <a name="xamarinforms-localization"></a>Xamarin.Forms 지역화

기본 제공되는 .NET 지역화 프레임워크는 Xamarin.Forms로 플랫폼 간 다국어 애플리케이션을 빌드하는 데 사용할 수 있습니다.

## <a name="xamarinforms-string-and-image-localizationtextmd"></a>[Xamarin.Forms 문자열 및 이미지 지역화](text.md)

.NET 애플리케이션 지역화를 위해 기본 제공되는 메커니즘에서는 [RESX 파일](https://docs.microsoft.com/dotnet/framework/resources/creating-resource-files-for-desktop-apps#resources-in-resx-files)과 `System.Resources` 및 `System.Globalization` 네임스페이스의 클래스를 사용합니다. 변환된 문자열이 포함된 RESX 파일이 변환에 대한 강력한 형식의 액세스 권한을 제공하는 컴파일러 생성 클래스와 함께 Xamarin.Forms 어셈블리에 포함됩니다. 그러면 변환된 텍스트를 코드에서 검색할 수 있습니다.

## <a name="right-to-left-localization"></a>[오른쪽에서 왼쪽으로 지역화](right-to-left.md)

흐름 방향은 페이지의 UI 요소를 육안으로 흝어보는 방향입니다. 오른쪽에서 왼쪽으로 쓰는 언어 지역화는 Xamarin.Forms 애플리케이션에 오른쪽에서 왼쪽 흐름 방향을 추가로 지원합니다.
