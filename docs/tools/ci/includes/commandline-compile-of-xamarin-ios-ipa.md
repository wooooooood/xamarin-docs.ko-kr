---
ms.openlocfilehash: 31c8d1b781648f2d9c69062c52e71478fc7a496b
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69529132"
---

IPhone 용 **SOLUTION_FILE** 솔루션의 릴리스 빌드를 지정 하려면 다음 명령줄을 지정 합니다. 명령줄에서 속성을 `IpaPackageDir` 지정 하 여 IPA의 위치를 설정할 수 있습니다.

- Mac에서 **xbuild**를 사용 합니다.

  ```
  xbuild /p:Configuration="Release" \ 
         /p:Platform="iPhone" \ 
         /p:IpaPackageDir="$HOME/Builds" \
         /t:Build MyProject.sln
  ```

**Xbuild** 명령은 일반적으로 **/Library/Frameworks/Mono.framework/Commands**디렉터리에 있습니다.

- Windows에서 **msbuild**를 사용 합니다.

  ```
  msbuild /p:Configuration="Release" 
          /p:Platform="iPhone" 
          /p:IpaPackageDir="%USERPROFILE%\Builds" 
          /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser"  
          /t:Build MyProject.sln
  ```

**msbuild** 는 명령줄에서 전달 `$( )` 된 식을 자동으로 확장 하지 않습니다. 따라서 명령줄에서을 `IpaPackageDir` 설정할 때 전체 경로를 사용 하는 것이 좋습니다.

`IpaPackageDir` 속성에 대 한 자세한 내용은 [iOS 9.8에 대 한 릴리스 정보](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/xamarin.ios_9/xamarin.ios_9.8.md#new-msbuild-property-ipapackagedir-to-customize-ipa-output-location) 를 참조 하세요.
