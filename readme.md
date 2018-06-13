# MyXamarinFormsTemplate
## 1. テンプレートの仕様  
  * Xamarin.Forms 2.5.1
  * .NET Standard 2.0
## 2. 当リポジトリの構成
  * src　：　テンプレートの元となっているソリューション
  * template　：　以下のテンプレート作成方法に沿って作成したテンプレートのソリューションやZIPファイル  
## 3. 参考情報
* [【Visual Studio】共有プロジェクトを含むマルチプロジェクトテンプレートの作り方](https://chronoir.net/create-multi-project-template-include-shared-project/)
* https://github.com/ytabuchi?tab=repositories
## 4. 作成方法
* ### 各プロジェクトファイルのテンプレート作成
  [【Visual Studio】共有プロジェクトを含むマルチプロジェクトテンプレートの作り方](https://chronoir.net/create-multi-project-template-include-shared-project/) の「エクスポートしたテンプレートファイル（zip）をそれぞれ解凍します。」までの作業は同じ。
* ### VSTemplateの作成  
  「My Exported Templates」フォルダにMyTemplate.vstemplateを作成し、以下の内容を書き込む。
``` xml
<VSTemplate Version="3.0.0" xmlns="http://schemas.microsoft.com/developer/vstemplate/2005" Type="ProjectGroup">
  <TemplateData>
    <Name>My Xamarin.Forms Template</Name>
    <Description>My Xamarin.Forms Template</Description>
    <ProjectType>CSharp</ProjectType>
    <ProjectSubType>
    </ProjectSubType>
    <SortOrder>1000</SortOrder>
    <CreateNewFolder>true</CreateNewFolder>
    <DefaultName>App</DefaultName>
    <ProvideDefaultName>true</ProvideDefaultName>
    <LocationField>Enabled</LocationField>
    <EnableLocationBrowseButton>true</EnableLocationBrowseButton>
    <CreateInPlace>true</CreateInPlace>
    <Icon>__TemplateIcon.ico</Icon>
    <PreviewImage></PreviewImage> 
  </TemplateData>
  <TemplateContent>
    <ProjectCollection>
		<ProjectTemplateLink ProjectName="$safeprojectname$">MyXamarinFormsTemplate\MyTemplate.vstemplate</ProjectTemplateLink>
		<ProjectTemplateLink ProjectName="$safeprojectname$.Android" CopyParameters="true">MyXamarinFormsTemplate.Android\MyTemplate.vstemplate</ProjectTemplateLink>
		<ProjectTemplateLink ProjectName="$safeprojectname$.iOS" CopyParameters="true">MyXamarinFormsTemplate.iOS\MyTemplate.vstemplate</ProjectTemplateLink>
	</ProjectCollection>
  </TemplateContent>
</VSTemplate>
```
* ### VSTemplate(PCL,Android,iOS用)
  PCL,Android,iOSのVSTemplateファイルの内容を変更する。  
以下を追記しない場合、共有プロジェクトを参照できず、プロジェクトの作成に失敗する原因となるらしい。
``` xml
<CreateInPlace>true</CreateInPlace>
```
* ### csproj
  Android,iOSの各csprojファイルを変更する。 
以下の通り変更することで各プロジェクトでPCLプロジェクトが参照されるようになる。  
$safeprojectname$ ではなく $<font color="Red">**ext_**</font>safeprojectname$ であることがポイント。  

変更前
``` xml
  <ItemGroup>
    <ProjectReference Include="..\MyXamarinFormsTemplate\MyXamarinFormsTemplate.csproj">
      <Project>{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</Project>
      <Name>MyXamarinFormsTemplate</Name>
    </ProjectReference>
  </ItemGroup>
  ```
  
変更後
``` xml
  <ItemGroup>
    <ProjectReference Include="..\$ext_safeprojectname$\$ext_safeprojectname$.csproj">
      <Project>{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}</Project>
      <Name>$ext_safeprojectname$</Name>
    </ProjectReference>
  </ItemGroup>
```
* ### Visual Studio への反映方法
  * Templateファイルを以下のパスに保存する。また、Visual Studioを立ち上げている場合はパスに保存後、再起動が必要となる。  
ツール＞オプション＞プロジェクトおよびソリューション＞場所＞ユーザー プロジェクト テンプレートの場所  
  * ファイル＞新規作成＞プロジェクトをクリックした際の表示される「新しいプロジェクト」画面で「Visual C#」の「Cross-Platform」などでも表示させたい場合は、上記のテンプレートの場所に「Cross-Platform」などのフォルダを作成し、テンプレートのZIPファイルを保存すると、「Cross-Platform」などでも表示され、選択できるようになる。
## 5. Templateの削除方法
「ユーザー プロジェクト テンプレートの場所」フォルダに保存されているテンプレートファイルを削除することで、インストールしたテンプレートは選択できないようになる。