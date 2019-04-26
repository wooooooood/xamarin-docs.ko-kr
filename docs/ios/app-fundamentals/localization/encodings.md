---
title: Xamarin.iOS의 국제화 인코딩
description: 이 문서에서는 Xamarin.iOS 사용할 수 있는 인코딩 및 앱에 추가 하는 방법에 설명의 국제화 인코딩을 설명 합니다.
ms.prod: xamarin
ms.assetid: F5117294-28BB-4583-B6A0-A339B050FDE1
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 04/28/2017
ms.openlocfilehash: db24c8677b0a3099193132575e92bc43a4c31dea
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61251035"
---
# <a name="internationalization-encodings-in-xamarinios"></a>Xamarin.iOS의 국제화 인코딩

일부 인코딩에 기본적으로 Xamarin.iOS 클래스 라이브러리에 포함 됩니다.

응용 프로그램의 크기를 줄이려면 Xamarin.iOS 특정 인코딩을 포함 하지 않습니다 하 고 mtouch 해야 인코딩에 대 한 지원이 포함 된 어셈블리를 포함 하도록 지시 해야 합니다.

Mac 또는 Visual Studio에 대 한 Visual Studio에서 iOS 빌드/고급 창에서 추가 인코딩을 선택 하면 됩니다.

 [![](encodings-images/00.png "추가 인코딩 선택")](encodings-images/00.png#lightbox)

 [![](encodings-images/00a.png "추가 인코딩 선택")](encodings-images/00a.png#lightbox)

다음 중 하나를 선택할 수 있습니다.

-  한 중일: Chineese, 일본어 및 한국어
-  mideast: 아랍어, 히브리어, 터키어 및 Latin5 합니다.
-  기타: 키릴 자모, 발트어, 베트남어, 우크라이나어 및 태국어
-  드물게 발생 합니다. EBCDIC 인코딩 및 다른 드문 코드 페이지
-  서 부: 라틴어, 이스터 및 유럽 서 부
-  모두


 <a name="cjk" />


## <a name="cjk"></a>cjk

-  CP51932
-  CP932
-  CP936
-  CP949
-  CP950
-  CP54936


 <a name="mideast" />


## <a name="mideast"></a>mideast

-  CP1254
-  CP1255
-  CP1256
-  CP28596
-  CP28598
-  CP28599
-  CP38598


 <a name="other" />


## <a name="other"></a>기타

-  CP1251
-  CP1257
-  CP1258
-  CP20866
-  CP21866
-  CP28594
-  CP28595
-  CP57002
-  CP874


 <a name="rare" />


## <a name="rare"></a>드물게 발생

-  CP1026
-  CP1047
-  CP1140
-  CP1141
-  CP1142
-  CP1143
-  CP1144
-  CP1145
-  CP1146
-  CP1147
-  CP1148
-  CP1149
-  CP20273
-  CP20277
-  CP20278
-  CP20280
-  CP20284
-  CP20285
-  CP20290
-  CP20297
-  CP20420
-  CP20424
-  CP20871
-  CP21025
-  CP37
-  CP500
-  CP708
-  CP852
-  CP855
-  CP857
-  CP858
-  CP862
-  CP864
-  CP866
-  CP869
-  CP870
-  CP875


 <a name="west" />


## <a name="west"></a>서 부

-  CP10000
-  CP10079
-  CP1250
-  CP1252
-  CP1253
-  CP28592
-  CP28593
-  CP28597
-  CP28605
-  CP437
-  CP850
-  CP860
-  CP861
-  CP863
-  CP865

