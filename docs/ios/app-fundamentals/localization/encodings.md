---
title: Xamarin.ios의 국제화 인코딩
description: 이 문서에서는 사용 가능한 인코딩과 앱에 추가 하는 방법에 대해 설명 하는 Xamarin.ios의 국제화 인코딩에 대해 설명 합니다.
ms.prod: xamarin
ms.assetid: F5117294-28BB-4583-B6A0-A339B050FDE1
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 04/28/2017
ms.openlocfilehash: c8e82f9261601db48ec48092a5f3f81394a86eec
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70763417"
---
# <a name="internationalization-encodings-in-xamarinios"></a>Xamarin.ios의 국제화 인코딩

모든 인코딩이 기본적으로 Xamarin.ios 클래스 라이브러리에 포함 되는 것은 아닙니다.

응용 프로그램의 크기를 줄이기 위해 Xamarin.ios에는 특정 인코딩이 포함 되지 않으며 필요한 인코딩에 대 한 지원이 포함 된 어셈블리를 포함 하도록 mtouch에 지시 해야 합니다.

이 작업을 수행 하려면 Mac용 Visual Studio 또는 Visual Studio의 iOS 빌드/고급 창에서 추가 인코딩을 선택 합니다.

 [![](encodings-images/00.png "추가 인코딩 선택")](encodings-images/00.png#lightbox)

 [![](encodings-images/00a.png "추가 인코딩 선택")](encodings-images/00a.png#lightbox)

다음 중 하나를 선택할 수 있습니다.

- cjk: Chineese, 일본어 및 한국어
- mideast: 아랍어, 히브리어, 터키어 및 Latin5.
- 다른 키릴 자모, 발트어, 베트남어, 우크라이나어 및 태국어
- 하지만 EBCDIC 인코딩 및 기타 드물게 발생 하는 코드 페이지
- 반원형 라틴 언어, 부활절 및 서유럽
- 모두

 <a name="cjk" />

## <a name="cjk"></a>cjk

- CP51932
- CP932
- CP936
- CP949
- CP950
- CP54936

 <a name="mideast" />

## <a name="mideast"></a>mideast

- CP1254
- CP1255
- CP1256
- CP28596
- CP28598
- CP28599
- CP38598

 <a name="other" />

## <a name="other"></a>기타

- CP1251
- CP1257
- CP1258
- CP20866
- CP21866
- CP28594
- CP28595
- CP57002
- CP874

 <a name="rare" />

## <a name="rare"></a>하지만

- CP1026
- CP1047
- CP1140
- CP1141
- CP1142
- CP1143
- CP1144
- CP1145
- CP1146
- CP1147
- CP1148
- CP1149
- CP20273
- CP20277
- CP20278
- CP20280
- CP20284
- CP20285
- CP20290
- CP20297
- CP20420
- CP20424
- CP20871
- CP21025
- CP37
- CP500
- CP708
- CP852
- CP855
- CP857
- CP858
- CP862
- CP864
- CP866
- CP869
- CP870
- CP875

 <a name="west" />

## <a name="west"></a>반원형

- CP10000
- CP10079
- CP1250
- CP1252
- CP1253
- CP28592
- CP28593
- CP28597
- CP28605
- CP437
- CP850
- CP860
- CP861
- CP863
- CP865
