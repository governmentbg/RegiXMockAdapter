# Примерна реализация на адаптер
Настоящият документ описва примерни стъпки за разработка на адаптер за RegiX.

## Съдържание

<!-- TOC -->

- [1. Създаване на проект](#heading)
  * [1.1. Промяна на DefaultNamespace](#sub-heading)
  * [1.2. Добавяне на описание на файлове в доставяния пакет](#sub-heading)
- [2. Добавяне на зависимости чрез инсталация на Nuget пакети](#heading-1)
  * [2.1. Създаване на nuspec файлове](#sub-heading-1)



    + [Sub-sub-heading](#sub-sub-heading-1)
- [4. Добавяне на схеми, метаданни, трансформации](#схеми-трансформации)
  * [Sub-heading](#sub-heading-2)
    + [Sub-sub-heading](#sub-sub-heading-2)


<!-- /TOC -->
## 1. Създаване на проект
В настоящата инструкция, за разработка на адаптерите се използва .NET Framework. Създайте три проекта със следните типове и примерни имена:

* RegiX.**Name**Adapter с тип Class Library (.NET Standard)
* RegiX.**Name**Adapter.Mock с тип Class Library (.NET Standard)
* RegiX.**Name**Adapter.Test с тип MSTest Test Project (.NET Core)

След създаване на проектите, премахнете автоматично създадените класове за всеки от създадените проекти.

### 1.1. Промяна на DefaultNamespace
Променете DefaultNamespace, така че да съдържа името на компанията разработчик:

  * **Company**.RegiX.**Name**Adapter
  * **Company**.RegiX.**Name**Adapter.Mock
  * **Company**.RegiX.**Name**Adapter.Test

Структурата на Namespace е: **Company**.RegiX.**Name**Adapter

Редактирайте описанието на проекта (auhtor, company, description) като натиснете десен бутон на мишката върху името на проекта, изберете Properties и то менюто в ляво Package.

### 1.2. Добавяне на описание на файлове в доставяния пакет
За всеки от проектите, натиснете върху наименованието му. Между таговете `</PropertyGroup> и </Project>` добавете:

* За адаптер **Company**.RegiX.**Name**Adapter

```xml
  <ItemGroup>
    <Content Include="XMLSamples\*\*.xml">
      <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
    </Content>
    <Content Include="XMLSchemas\*\Transformations\*.xslt">
      <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
    </Content>
    <Content Include="XMLSchemas\*\*.xsd">
      <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
    </Content>
    <Content Include="XMLMetaData\*\*.xml">
      <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
    </Content>
  </ItemGroup>
```

* За адаптер **Company**.RegiX.**Name**Adapter.Mock кода се добавя между `</PropertyGroup> и </Project>`.

```xml
  <ItemGroup>
    <Content Include="XMLData\*\*.xml">
      <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
    </Content>
  </ItemGroup>
```

## 2. Добавяне на зависимости чрез инсталация на Nuget пакети

### 2.1. Създаване на nuspec файлове 
За работа по създадените проекти с Nuget Packet Manager, необходимо е да генерирате XML метаописания. В директориите на проектите RegiX.**Name**Adapter и RegiX.**Name**Adapter.Mock изпълнете командата `nuget spec`, за да създадете .nuspec файлове. Последна версия на nuget.exe ще откриете на адрес: https://www.nuget.org/downloads.

Редактирайте съдържанието на .nuspec файловете `RegiX.**Name**Adapter.nuspec` и `RegiX.**Name**Adapter.Mock`, като изтриете елементите `<licenceURL>, <projectURL>, <iconURL>,<copyright2020>, <tags>`.

### 2.2. Разширяване структурата на проектите
В директорията на проект RegiX.**Name**Adapter, създайте следните поддиректории:
* AdapterService
* APIService
* XMLMetaData\RegiX.**Name**Adapter
* XMLSamples\RegiX.**Name**Adapter
* XMLSchemas\RegiX.**Name**Adapter

### 2.3. Добавяне на пакети

#### 2.3.1. Изтегляне на необходимите пакети, които ще бъдат инсталирани
  Добавете път към адрес на местоположение на Nuget пакети, като изберете Tools, Nuget Package Manager, Packet Manager Settings.
  От менюто в ляво се избира: Pacakage Sources. Добавянето на пътя към Nuget пакетите става чрез натискането на бутона плюс и след това попълването на Името и в полето Source се посочва пътя към пакетите. 

#### 2.3.2. Инсталиране на пакети
След задаване на адрес на хранилище с Nuget пакети, е необходимо те да бъдат инсталирани. За проектите добавете следните референции, като натиснете десен бутон върху името на проекта и изберете Manage NuGet Packages от контекстното меню:

* **RegiX.Adapters.Common** в проект RegiX.**Name**Adapter
* **RegiX.Adapters.Mocks** в проект RegiX.**Name**Adapter.Mock
* **RegiX.Adapters.TestUtils** в проект RegiX.**Name**Adapter.Test

## 3. Създаване на класове

Създайте спрямо описанието в стандарта за раработка на адаптери следние файлове, които да служат като класове, в съответните директории.  

`RegiX.**Name**Adapter\RegiX.**Name**Adapter\AdapterService\I**Name**Adapter.cs`
`RegiX.**Name**Adapter\RegiX.**Name**Adapter\AdapterService\NameAdapter.cs`
`RegiX.**Name**Adapter\RegiX.**Name**Adapter\APIService\I**Name**API.cs`
`RegiX.**Name**Adapter\RegiX.**Name**Adapter\APIService\**Name**API.cs`

### 3.1. Добавяне на методи (операции) в класове

Във всеки от създадените в т. 3 класове е необходимо да въведете метод за всяка операция. Кодът използван по-долу е примерен. Необходимо е да промените `namespace` според наименованието на вашите организация и име на проект.

* В I**Name**Adapter.cs използвайте:

```csharp
using System.ComponentModel;
using System.ServiceModel;
using TechnoLogica.RegiX.Common;
using TechnoLogica.RegiX.Common.DataContracts;
using TechnoLogica.RegiX.Common.ObjectMapping;

namespace DAEU.RegiX.SampleAdapter.AdapterService
{
    [ServiceContract]
    [Description("Примерен адаптер")]
    public interface ISampleAdapter : IAdapterServiceWCF
    {
        [OperationContract]
        [Description("Примерна услуга 1")]
        CommonSignedResponse<ExampleRequest, ExampleResponse> Example(ExampleRequest argument, AccessMatrix accessMatrix, AdapterAdditionalParameters aditionalParameters);

        [OperationContract]
        [Description("Примерна услуга 2")]
        CommonSignedResponse<ExampleRequest, ExampleResponse> Example2(ExampleRequest argument, AccessMatrix accessMatrix, AdapterAdditionalParameters aditionalParameters);

        [OperationContract]
        [Description("Примерна услуга 3")]
        CommonSignedResponse<ExampleRequest, ExampleResponse> Example3(ExampleRequest argument, AccessMatrix accessMatrix, AdapterAdditionalParameters aditionalParameters);
    }
}
```

* В **Name**Adapter.cs използвайте:

```csharp
using System.ComponentModel.Composition;
using TechnoLogica.RegiX.Adapters.Common;
using TechnoLogica.RegiX.Adapters.Common.DataContracts;
using TechnoLogica.RegiX.Adapters.Common.ExportExtension;
using TechnoLogica.RegiX.Adapters.Common.ObjectMapping;
using TechnoLogica.RegiX.Common;
using TechnoLogica.RegiX.Common.DataContracts;
using TechnoLogica.RegiX.Common.DataContracts.Parameter;
using TechnoLogica.RegiX.Common.ObjectMapping;
using TechnoLogica.RegiX.Common.Utils;

namespace DAEU.RegiX.SampleAdapter.AdapterService
{
    [Export(typeof(IAdapterService))]
    [ExportFullName(typeof(SampleAdapter), typeof(IAdapterService))]
    [ExportSimpleName(typeof(SampleAdapter), typeof(IAdapterService))]
    public class SampleAdapter : BaseAdapterService, ISampleAdapter
    {
        public CommonSignedResponse<ExampleRequest, ExampleResponse> Example(ExampleRequest argument, AccessMatrix accessMatrix, AdapterAdditionalParameters aditionalParameters)
        {
            ExampleResponse response = new ExampleResponse();
            response.StringData = argument.Identifier;
            response.StringData2 = "Operation 1";
            
            var mapper = new SelfMapper<ExampleResponse>(accessMatrix);
            ExampleResponse result = new ExampleResponse();
            mapper.Map(response, result);

            return SigningUtils.CreateAndSign(argument, result, accessMatrix, aditionalParameters);
        }

        public CommonSignedResponse<ExampleRequest, ExampleResponse> Example2(ExampleRequest argument, AccessMatrix accessMatrix, AdapterAdditionalParameters aditionalParameters)
        {
            ExampleResponse response = new ExampleResponse();
            response.StringData = argument.Identifier;
            response.StringData2 = "Operation 2";

            var mapper = new SelfMapper<ExampleResponse>(accessMatrix);
            ExampleResponse result = new ExampleResponse();
            mapper.Map(response, result);

            return SigningUtils.CreateAndSign(argument, result, accessMatrix, aditionalParameters);
        }

        public CommonSignedResponse<ExampleRequest, ExampleResponse> Example3(ExampleRequest argument, AccessMatrix accessMatrix, AdapterAdditionalParameters aditionalParameters)
        {
            ExampleResponse response = new ExampleResponse();
            response.StringData = argument.Identifier;
            response.StringData2 = "Operation 3";

            var mapper = new SelfMapper<ExampleResponse>(accessMatrix);
            ExampleResponse result = new ExampleResponse();
            mapper.Map(response, result);

            return SigningUtils.CreateAndSign(argument, result, accessMatrix, aditionalParameters);
        }
    }
}
```

* **Name**API.cs

```csharp
using DAEU.RegiX.SampleAdapter.AdapterService;
using System.ComponentModel.Composition;
using TechnoLogica.RegiX.Adapters.Common;
using TechnoLogica.RegiX.Adapters.Common.ExportExtension;
using TechnoLogica.RegiX.Common;
using TechnoLogica.RegiX.Common.TransportObjects;

namespace DAEU.RegiX.SampleAdapter.APIService
{
    [ExportFullName(typeof(ISampleAPI), typeof(IAPIService))]
    [Export(typeof(IAPIService))]
    public class SampleAPI : BaseAPIService, ISampleAPI
    {
        public ServiceResultDataSigned<ExampleRequest, ExampleResponse> Example(ServiceRequestData<ExampleRequest> argument)
        {
            return AdapterClient.Execute<ISampleAdapter, ExampleRequest, ExampleResponse>(
                (i, r, a, o) => i.Example(r, a, o),
                argument);
        }

        public ServiceResultDataSigned<ExampleRequest, ExampleResponse> Example2(ServiceRequestData<ExampleRequest> argument)
        {
            return AdapterClient.Execute<ISampleAdapter, ExampleRequest, ExampleResponse>(
                (i, r, a, o) => i.Example2(r, a, o),
                argument);
        }

        public ServiceResultDataSigned<ExampleRequest, ExampleResponse> Example3(ServiceRequestData<ExampleRequest> argument)
        {
            return AdapterClient.Execute<ISampleAdapter, ExampleRequest, ExampleResponse>(
                (i, r, a, o) => i.Example3(r, a, o),
                argument);
        }
    }
}
```

* I**Name**API.cs

```csharp
using System.ComponentModel;
using System.ServiceModel;
using TechnoLogica.RegiX.Adapters.Common.Attributes;
using TechnoLogica.RegiX.Common;
using TechnoLogica.RegiX.Common.TransportObjects;

namespace DAEU.RegiX.SampleAdapter.APIService
{
    [ServiceContract]
    [XmlSerializerFormat]
    [Description("API на примерен адаптер")]
    public interface ISampleAPI : IAPIService
    {
        [OperationContract]
        [Description("Изпълнява примерна услуга 1")]
        ServiceResultDataSigned<ExampleRequest, ExampleResponse> Example(ServiceRequestData<ExampleRequest> argument);

        [OperationContract]
        [Description("Изпълнява примерна услуга 2")]
        [Info(requestXSD: "ExampleRequest.xsd", responseXSD: "ExampleResponse.xsd",
            sampleRequest: "ExampleRequest.xml",
            sampleResponse: "ExampleResponse.xml",
            requestXSLT: "ExampleRequest.xslt",
            responseXSLT: "ExampleResponse.xslt", metaDataXML: "Example.xml")]
        ServiceResultDataSigned<ExampleRequest, ExampleResponse> Example2(ServiceRequestData<ExampleRequest> argument);

        [OperationContract]
        [Description("Изпълнява примерна услуга 3")]
        [Info(requestXSD: "ExampleRequest.xsd", responseXSD: "ExampleResponse.xsd",
            sampleRequest: "ExampleRequest.xml",
            sampleResponse: "ExampleResponse.xml",
            requestXSLT: "ExampleRequest.xslt",
            responseXSLT: "ExampleResponse.xslt", metaDataXML: "Example.xml")]
        ServiceResultDataSigned<ExampleRequest, ExampleResponse> Example3(ServiceRequestData<ExampleRequest> argument);
    }
}
```

Класовете I**Name**API.cs и **Name**API.cs работят на ядрото на RegiX. I**Name**Adapter.cs и NameAdapter.cs работят при регистъра, за който разработваме адаптер.

## 4. Добавяне на схеми, метаданни, трансформации
Във всеки от класовете е необходимо да въведете метод за всяка операция. Можете да генерирате класове от xml схеми чрез инструмента Xml2Code или да използвате примерния код по-долу. Необходимо е да промените връзките и `namespace`, според наименованието на вашите организация и име на проект и да добавите допълнителни операции.

### 4.1. Попълване на схеми, метаданни и трансформации

`RegiX.**Name**Adapter\RegiX.**Name**Adapter\XMLSchemas\RegiX.**Name**Adapter\ExampleRequest.xsd`
`RegiX.**Name**Adapter\RegiX.**Name**Adapter\XMLSchemas\RegiX.**Name**Adapter\ExampleResponse.xsd`
`RegiX.**Name**Adapter\RegiX.**Name**Adapter\XMLSchemas\RegiX.**Name**Adapter\ExampleRequest.designer.cs`
`RegiX.**Name**Adapter\RegiX.**Name**Adapter\XMLSchemas\RegiX.**Name**Adapter\ExampleResponse.designer.cs`

`RegiX.**Name**Adapter\RegiX.**Name**Adapter\XMLSchemas\RegiX.**Name**Adapter\Transformations\ExampleRequest.sps`
`RegiX.**Name**Adapter\RegiX.**Name**Adapter\XMLSchemas\RegiX.**Name**Adapter\Transformations\ExampleResponse.sps`

`RegiX.**Name**Adapter\RegiX.**Name**Adapter\XMLSchemas\RegiX.**Name**Adapter\Transformations\ExampleRequest.xslt`
`RegiX.**Name**Adapter\RegiX.**Name**Adapter\XMLSchemas\RegiX.**Name**Adapter\Transformations\ExampleResponse.xml`
`RegiX.**Name**Adapter\RegiX.**Name**Adapter\XMLSchemas\RegiX.**Name**Adapter\Transformations\ExampleResponse.xslt`

`RegiX.**Name**Adapter\RegiX.**Name**Adapter\XMLMetaData\RegiX.**Name**Adapter\Example.xml`

`RegiX.**Name**Adapter\RegiX.**Name**Adapter\XMLSamples\RegiX.**Name**Adapter\ExampleRequest.xml`
`RegiX.**Name**Adapter\RegiX.**Name**Adapter\XMLSamples\RegiX.**Name**Adapter\ExampleResponse.xml`

### 4.2. Добавяне на референции

Във SampleTests.cs --> AdapterService --> добавя референция към Technologica.RegiX.Common; 
                                          Add reference to Regix.SampleAdapter;
                      AdapterMockTest --> add reference to RegiX.SampleAdapter.Mock;

                      SampleAdapterAPITest.cs --> using DAEU.RegiXSampleAdapter.AdapterService;

### 4.3. Дебъг и допълване на структура и namespace                                                       

Необходимо е да изпълните инструкциите добавяне на 
<ItemGroup>
    <Content Include="XMLSamples\*\*.xml">
      <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
    </Content>
    <Content Include="XMLSchemas\*\Transformations\*.xslt">
      <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
    </Content>
    <Content Include="XMLSchemas\*\*.xsd">
      <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
    </Content>
    <Content Include="XMLMetaData\*\*.xml">
      <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
    </Content>
  </ItemGroup>
към проекта RegiX.ReferenceAdapter (double click на проекта във Visual studio го отваря в текстов файл)
и 
  <ItemGroup>
    <Content Include="XMLData\*\*.xml">
      <CopyToOutputDirectory>PreserveNewest</CopyToOutputDirectory>
    </Content>
  </ItemGroup>

към проекта RegiX.ReferenceAdapter.Mock

Освен това трябва да се промени namepsace-а на Mock резултатите (тези в папката RegiX.DaeuTestAdapter\RegiX.DaeuTestAdapter.Mock\XMLData\RegiX.DaeuTestAdapter.Mock) от
от http://egov.bg/RegiX/Sample/ExampleResponse на  http://egov.bg/RegiX/DaeuTest/ExampleResponse

### 4.4. Генериране на nuget пакети

Properties на проекта --> Build --> Generate a nuget packet on build
Изпълнява се за двата проекта

## 5. Създаване на .NET Core конзолно приложение

Във Visual Studio създайте ново решение (solution) и проект на ново конзолно приложение тип Console App (.NET Core), с име: RegiX.*Name*Adapter.NetCoreHost.

### 5.1. Добавяне на nuget пакети:

Следните пакети се добавят към проекта на новото .NET Core конзолно приложение:

* RegiX.ReferenceAdapter -- генериран при изграждане (build) на проекта, описан в т. 4;
* RegiX.Adapters.NetCoreAdapterHost
* RegiX.Adapters.NetCoreParameterStore
* RegiX.SecureBlackBox.CertFinder.File
* RegiX.SecureBlackBoxSigner.NetCore

При инсталация, в NuGet Package Manager поставете отметка в полето Include Prerelease.
За успешна инсталация на посочените пакети е необходимо в Package Manager да имате добавена връзка към хранилището nuget.org, с адрес: https://api.nuget.org/v3/index.json

### 5.2. Добавяне на сертификат 

Добавете сертификат в следната под-папка на проекта:

\bin\Debug\netcoreapp3.1\RegiX3Certificate.pfx
За изпълнение на тази стъпка е необходимо да си осигурите сертификат за цифрово подпечатване на xml документи.

### 5.3. Добавяне на конфигурационен и скриптов файл

С десен бутон върху проекта, изберете Add и добавете TypeScript JSON Configuration File с име 'settings'.

Добавете и Application Configuration File със следното съдържание

````xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <configSections>
    <section name="log4net" type="log4net.Config.Log4NetConfigurationSectionHandler,Log4net"/>
  </configSections>
  <appSettings>
    <add key="SignResponse" value="true" />
    <add key="CertificateFile" value="RegiXCertificate.pfx" />
    <add key="CertificatePassword" value="P@ssw0rd" />
    <add key="TimestampServer" value="http://tsa.swisssign.net" />
    <add key="SecureBlackBoxLicenseKey" value="90A171B84E81136ED1EA745564B7F84047A13024C91781A1BBF521A83C83F6754795BE83093EA8229B58BB8416984040C2A69D2DA25250232B33F67A65CA4449C2C49D084964F14E84ADE98C2D4FE67B6D44A2446E43FAE599D83FF1BC29662C73CD106A1B183CEC0C4688C8B94F6C41ACBB26D4C22DB37CE53DE1C2C7B9031759AB74ACFC273328D65D5B4A2C2BCFC5554CA3A501C75E9B9BA564934D58E740AF0C2E37560DD6B890CF290897DEB6F32048F3BC5E9781DDAEF7F1D5AD8EC2A5912C3FFFB82B5A657091AAC6E589DCB4EA93AFEFCBBBA3AC127AFAE53A780F512A868574D0E2DBB311B35D707EE8AA8710A3062FC798E2804E39178C6073E01F" />
    <!--<add key="LoggingAddress" value="http://localhost:8080/"/>-->
    <!--<add key="RabbitHostName" value="localhost"/>-->
  </appSettings>
  <log4net debug="false">
    <appender name="ConsoleAppender" type="log4net.Appender.ConsoleAppender">
      <layout type="log4net.Layout.PatternLayout">
        <conversionPattern value="%date [%thread] %-5level %logger [%property{NDC}] - %message%newline" />
      </layout>
    </appender>
    <root>
      <level value="ALL" />
      <appender-ref ref="ConsoleAppender" />
    </root>
  </log4net>
</configuration>
````

В следните тагове въведете името и паролата на вашият сертификат

``xml 
    <add key="CertificateFile" value="certificate.pfx" />
    <add key="CertificatePassword" value="P@ssw0rd" />
``

### 5.4. Изграждане и стартиране

Във Visual Studio изпълняваме Build and run.

### 5.5. Тест през Visual Studio Code

От Extension Market Place във VS Code, потърсете и инсталирайте Rest Client.
