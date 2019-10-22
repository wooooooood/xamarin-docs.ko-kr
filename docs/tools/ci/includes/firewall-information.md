---
ms.openlocfilehash: d46968f9c53314abe561e7f4871cfbf6e07b7002
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/21/2019
ms.locfileid: "61047856"
---
## <a name="firewall-configuration"></a>방화벽 구성

Xamarin Test Cloud에 테스트를 제출 하려면 테스트를 제출 하는 컴퓨터에서 Test Cloud 서버와 통신할 수 있어야 합니다. 포트 80 및 443에서 **testcloud.xamarin.com** 에 위치한 서버와의 네트워크 트래픽을 허용 하도록 방화벽을 구성 해야 합니다. 이 끝점은 DNS를 통해 관리 되며 IP 주소는 변경 될 수 있습니다. 

경우에 따라 테스트 또는 테스트를 실행 하는 장치는 방화벽으로 보호 되는 웹 서버와 통신 해야 합니다. 이 시나리오에서는 다음 IP 주소에서 트래픽을 허용 하도록 방화벽을 구성 해야 합니다.

* **195.249.159.238**
* **195.249.159.239**
