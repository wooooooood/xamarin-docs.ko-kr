---
ms.openlocfilehash: 05f1017f8c4b306996d3e8e165511ff9062a1026
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61047861"
---

솔루션의 릴리스 빌드를 지정 하려면 다음 명령줄 **SOLUTION_FILE.sln** iPhone 용입니다. IPA 위치를 지정 하 여 설정할 수는 `IpaPackageDir` 명령줄에서 속성:

 - Mac에서 사용 하 여 **xbuild**:

        xbuild /p:Configuration="Release" \ 
           /p:Platform="iPhone" \ 
           /p:IpaPackageDir="$HOME/Builds" \
           /t:Build MyProject.sln

합니다 **xbuild** 명령은 일반적으로 디렉터리에 있습니다 **/Library/Frameworks/Mono.framework/Commands**합니다.

 - Windows를 사용 하 여 온 **msbuild**:

        msbuild /p:Configuration="Release" 
            /p:Platform="iPhone" 
            /p:IpaPackageDir="%USERPROFILE%\Builds" 
            /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser"  
            /t:Build MyProject.sln


**msbuild** 자동으로 확장 되지 것입니다 `$( )` 명령줄에 의해 전달 된 식입니다. 이러한 이유로 것이 좋습니다 설정 하는 경우 전체 경로 사용 하는 `IpaPackageDir` 명령줄에서.


참조를 [9.8 iOS에 대 한 릴리스 정보](https://developer.xamarin.com/releases/ios/xamarin.ios_9/xamarin.ios_9.8/#New_MSBuild_property_IpaPackageDir_to_customize_.ipa_output_location) 대 한 자세한 내용은 `IpaPackageDir` 속성입니다.
