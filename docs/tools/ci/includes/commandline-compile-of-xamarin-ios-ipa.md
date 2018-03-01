
솔루션의 릴리스 빌드를 지정 하려면 다음 명령줄 **SOLUTION_FILE.sln** iPhone에 대 한 합니다. IPA의 위치를 지정 하 여 설정할 수 있습니다는 `IpaPackageDir` 명령줄에서 속성:

 - Mac에서 사용 하 여 **xbuild**:

        xbuild /p:Configuration="Release" \ 
           /p:Platform="iPhone" \ 
           /p:IpaPackageDir="$HOME/Builds" \
           /t:Build MyProject.sln

**xbuild** 명령은 일반적으로 디렉터리에 있습니다 **/Library/Frameworks/Mono.framework/Commands**합니다.

 - Windows에서 사용 하 여 **msbuild**:

        msbuild /p:Configuration="Release" 
            /p:Platform="iPhone" 
            /p:IpaPackageDir="%USERPROFILE%\Builds" 
            /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser"  
            /t:Build MyProject.sln


**msbuild** 자동으로 확장 되지 것입니다 `$( )` 명령줄에 의해 전달 된 식입니다. 이러한 이유로 것을 설정할 때 전체 경로 사용 하 여 `IpaPackageDir` 명령줄에서.


참조는 [9.8 iOS에 대 한 릴리스 정보](https://developer.xamarin.com/releases/ios/xamarin.ios_9/xamarin.ios_9.8/#New_MSBuild_property_IpaPackageDir_to_customize_.ipa_output_location) 에 대 한 자세한 내용은 `IpaPackageDir` 속성입니다.
