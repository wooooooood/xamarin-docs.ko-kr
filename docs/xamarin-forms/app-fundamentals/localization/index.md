---
title: Xamarin.Forms 지역화
description: 기본 제공되는 .NET 지역화 프레임워크는 Xamarin.Forms로 플랫폼 간 다국어 애플리케이션을 빌드하는 데 사용할 수 있습니다. 텍스트 및 이미지를 지역화할 수 있고 애플리케이션은 오른쪽에서 왼쪽 흐름 방향을 지원할 수 있습니다.
ms.prod: xamarin
ms.assetid: 97BF843B-BDAA-4CEA-8189-6DB54B291D7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2018
ms.openlocfilehash: 71033e935a2d3a4be88dbcc5d975938771484640
ms.sourcegitcommit: b60a37587aad8a0bfa8a522d88d22fa672002443
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285575"
---
# <a name="xamarinforms-localization"></a>Xamarin.Forms 지역화

_기본 제공되는 .NET 지역화 프레임워크는 Xamarin.Forms로 플랫폼 간 다국어 애플리케이션을 빌드하는 데 사용할 수 있습니다._

## <a name="string-and-image-localizationtextmd"></a>[문자열 및 이미지 지역화](text.md)

.NET 애플리케이션 지역화를 위해 기본 제공되는 메커니즘에는 [RESX 파일](https://docs.microsoft.com/dotnet/framework/resources/creating-resource-files-for-desktop-apps#resources-in-resx-files)과 `System.Resources` 및 `System.Globalization` 네임스페이스의 클래스가 사용됩니다. 번역된 문자열이 포함된 RESX 파일이 번역에 대한 강력한 형식의 액세스를 제공하는 컴파일러 생성 클래스와 함께 Xamarin.Forms 어셈블리에 포함됩니다. 그러면 코드에서 번역된 텍스트를 검색할 수 있습니다.

## <a name="right-to-left-localizationright-to-leftmd"></a>[오른쪽에서 왼쪽으로 지역화](right-to-left.md)

흐름 방향은 페이지의 UI 요소를 육안으로 흝어보는 방향입니다. 오른쪽에서 왼쪽으로 지역화는 Xamarin.Forms 애플리케이션에 오른쪽에서 왼쪽 흐름 방향에 대한 지원을 추가합니다.
