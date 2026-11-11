
Источник [https://github.com/Bouncer77/soap-producer-web-service](https://github.com/Bouncer77/soap-producer-web-service).

Java 8:

```shell
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64
```

#### Получить WSDL файл 

URL [http://localhost:8080/ws/countries.wsdl](http://localhost:8080/ws/countries.wsdl)

#### Пример запроса с параметром из файла

```shell
curl --header "content-type: text/xml" -d @request_spain.xml http://localhost:8080/ws
```

__Без__ информации о скорости и т.п. характеристиках, только тело ответа(флаг -s), но с форматированием:

````shell
curl -s --header "content-type: text/xml" -d @request_spain.xml http://localhost:8080/ws  | xmlstarlet fo
````

Результат:

````xml
<?xml version="1.0"?>
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/">
  <SOAP-ENV:Header/>
  <SOAP-ENV:Body>
    <ns2:getCountryResponse xmlns:ns2="http://spring.io/guides/gs-producing-web-service">
      <ns2:country>
        <ns2:name>Spain</ns2:name>
        <ns2:population>46704314</ns2:population>
        <ns2:capital>Madrid</ns2:capital>
        <ns2:currency>EUR</ns2:currency>
      </ns2:country>
    </ns2:getCountryResponse>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
````

Запрос с curl:

````shell
curl -s --header "content-type: text/xml" http://localhost:8080/ws/countries.wsdl
````

или вариант с __httpie__ и выводом в файл:

```shell
http http://localhost:8080/ws/countries.wsdl > test1_result.xml
```

Ответ test1_result.xml:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?><wsdl:definitions xmlns:wsdl="http://schemas.xmlsoap.org/wsdl/" xmlns:sch="http://spring.io/guides/gs-producing-web-service" xmlns:soap="http://schemas.xmlsoap.org/wsdl/soap/" xmlns:tns="http://spring.io/guides/gs-producing-web-service" targetNamespace="http://spring.io/guides/gs-producing-web-service">
  <wsdl:types>
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified" targetNamespace="http://spring.io/guides/gs-producing-web-service">

    <xs:element name="getCountryRequest">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="name" type="xs:string"/>
            </xs:sequence>
        </xs:complexType>
    </xs:element>

    <xs:element name="getCountryResponse">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="country" type="tns:country"/>
            </xs:sequence>
        </xs:complexType>
    </xs:element>

    <xs:complexType name="country">
        <xs:sequence>
            <xs:element name="name" type="xs:string"/>
            <xs:element name="population" type="xs:int"/>
            <xs:element name="capital" type="xs:string"/>
            <xs:element name="currency" type="tns:currency"/>
        </xs:sequence>
    </xs:complexType>

    <xs:simpleType name="currency">
        <xs:restriction base="xs:string">
            <xs:enumeration value="GBP"/>
            <xs:enumeration value="EUR"/>
            <xs:enumeration value="PLN"/>
        </xs:restriction>
    </xs:simpleType>
</xs:schema>
  </wsdl:types>
  <wsdl:message name="getCountryResponse">
    <wsdl:part element="tns:getCountryResponse" name="getCountryResponse">
    </wsdl:part>
  </wsdl:message>
  <wsdl:message name="getCountryRequest">
    <wsdl:part element="tns:getCountryRequest" name="getCountryRequest">
    </wsdl:part>
  </wsdl:message>
  <wsdl:portType name="CountriesPort">
    <wsdl:operation name="getCountry">
      <wsdl:input message="tns:getCountryRequest" name="getCountryRequest">
    </wsdl:input>
      <wsdl:output message="tns:getCountryResponse" name="getCountryResponse">
    </wsdl:output>
    </wsdl:operation>
  </wsdl:portType>
  <wsdl:binding name="CountriesPortSoap11" type="tns:CountriesPort">
    <soap:binding style="document" transport="http://schemas.xmlsoap.org/soap/http"/>
    <wsdl:operation name="getCountry">
      <soap:operation soapAction=""/>
      <wsdl:input name="getCountryRequest">
        <soap:body use="literal"/>
      </wsdl:input>
      <wsdl:output name="getCountryResponse">
        <soap:body use="literal"/>
      </wsdl:output>
    </wsdl:operation>
  </wsdl:binding>
  <wsdl:service name="CountriesPortService">
    <wsdl:port binding="tns:CountriesPortSoap11" name="CountriesPortSoap11">
      <soap:address location="http://localhost:8080/ws"/>
    </wsdl:port>
  </wsdl:service>
</wsdl:definitions>
```

### xml_pp

```shell
curl -s --header "content-type: text/xml" -d @request_spain.xml http://localhost:8080/ws  | xml_pp

```

### Запросы с параметром

#### Запрос для Испании

Параметр запроса __request_spain.xml__:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:gs="http://spring.io/guides/gs-producing-web-service">
    <soapenv:Header/>
    <soapenv:Body>
        <gs:getCountryRequest>
            <gs:name>Spain</gs:name>
        </gs:getCountryRequest>
    </soapenv:Body>
</soapenv:Envelope>
```

Запрос с этим параметром:

```shell
curl -s --header "content-type: text/xml" -d @request_spain.xml http://localhost:8080/ws  | xml_pp
```
Ответ:

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/">
  <SOAP-ENV:Header/>
  <SOAP-ENV:Body>
    <ns2:getCountryResponse xmlns:ns2="http://spring.io/guides/gs-producing-web-service">
      <ns2:country>
        <ns2:name>Spain</ns2:name>
        <ns2:population>46704314</ns2:population>
        <ns2:capital>Madrid</ns2:capital>
        <ns2:currency>EUR</ns2:currency>
      </ns2:country>
    </ns2:getCountryResponse>
  </SOAP-ENV:Body>
</SOAP-ENV:Envelope>

```

#### Запрос для Польши

Параметр запроса __request_poland.xml__:

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:gs="http://spring.io/guides/gs-producing-web-service">
    <soapenv:Header/>
    <soapenv:Body>
        <gs:getCountryRequest>
            <gs:name>Poland</gs:name>
        </gs:getCountryRequest>
    </soapenv:Body>
</soapenv:Envelope>
```

Запрос с этим параметром:

```shell
curl -s --header "content-type: text/xml" -d @request_poland.xml http://localhost:8080/ws  | xml_pp
```

Ответ:

```xml
<SOAP-ENV:Envelope xmlns:SOAP-ENV="http://schemas.xmlsoap.org/soap/envelope/">
    <SOAP-ENV:Header/>
    <SOAP-ENV:Body>
        <ns2:getCountryResponse xmlns:ns2="http://spring.io/guides/gs-producing-web-service">
            <ns2:country>
                <ns2:name>Poland</ns2:name>
                <ns2:population>38186860</ns2:population>
                <ns2:capital>Warsaw</ns2:capital>
                <ns2:currency>PLN</ns2:currency>
            </ns2:country>
        </ns2:getCountryResponse>
    </SOAP-ENV:Body>
</SOAP-ENV:Envelope>
```

# Создание веб-службы SOAP

Проект является демонстрационным вариантом создания веб-службы SOAP при помощи
**Java 8** и фреймворка **Spring** и **Apache Maven**.

## Зависимости

№     | Название (ссылка)            | Версия | Описание
------|------------------------------|--------|--------------------------------
1     | [Spring Boot Starter Parent](https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-parent) | [2.6.4](https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-parent/2.6.4) | Родительский pom, обеспечивающий управление зависимостями и плагинами для приложений, созданных с помощью Maven.
2     | [Spring Boot Starter Web](https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web) | [2.6.4](https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web/2.6.4) | Стартер для создания веб-приложений, в том числе RESTful, с использованием Spring MVC. Использует Tomcat в качестве встроенного контейнера по умолчанию.
3     | [Spring Web Services](https://mvnrepository.com/artifact/org.springframework.ws/spring-ws/) | [3.1.2](https://mvnrepository.com/artifact/org.springframework.ws/spring-ws/3.1.2) | Поддержка веб-сервисов на основе Spring
4     | [Spring Boot Starter Test](https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-test/) | [2.6.4](https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-test/2.6.4) | Стартер для тестирования приложений Spring Boot с библиотеками, включая JUnit Jupiter, Hamcrest и Mockito.
5     | [Spring WS Core](https://mvnrepository.com/artifact/org.springframework.ws/spring-ws-core/) | [3.1.2](https://mvnrepository.com/artifact/org.springframework.ws/spring-ws-core/3.1.2) | Ядро Spring WS
6     | [WSDL4J](https://mvnrepository.com/artifact/wsdl4j/wsdl4j/) | [1.6.3](https://mvnrepository.com/artifact/wsdl4j/wsdl4j/1.6.3) | Генератор заглушек Java для WSDL
7     | [JavaBeans(TM) Activation Framework](https://mvnrepository.com/artifact/javax.activation/activation/) | [1.1.1](https://mvnrepository.com/artifact/javax.activation/activation/1.1.1) | Предоставляет API для создания и сборки сообщений SOAP.
8     | [JAXB API](https://mvnrepository.com/artifact/javax.xml.bind/jaxb-api/) | [2.3.1](https://mvnrepository.com/artifact/javax.xml.bind/jaxb-api/2.3.1) | JAXB предоставляет две основные возможности: маршаллирование Java объектов в XML и наоборот, то есть демаршализация из XML обратно в Java объект.
9     | [JAXB Runtime](https://mvnrepository.com/artifact/org.glassfish.jaxb/jaxb-runtime/) | [3.0.2](https://mvnrepository.com/artifact/org.glassfish.jaxb/jaxb-runtime/3.0.2) | JAXB (JSR 222) Эталонная реализация

## Плагины

№     | Название (ссылка)            | Версия | Описание
------|------------------------------|--------|--------------------------------
1     | [JAXB 2 Maven Plugin](https://mvnrepository.com/artifact/org.codehaus.mojo/jaxb2-maven-plugin) | [2.5.0](https://mvnrepository.com/artifact/org.codehaus.mojo/jaxb2-maven-plugin/2.5.0) | Плагин Mojo JAXB-2 Maven используется для создания графа объектов из XSD на основе реализации JAXB 2.x и для создания XSD из аннотированных классов Java JAXB.

## Тестирование

```bash
# Получить файл wsdl
http://localhost:8080/ws/countries.wsdl
```

# Use data from file

```shell
curl --header "content-type: text/xml" -d @request_spain.xml http://localhost:8080/ws
```

# Запрос с многострочным вводом

Внимание на __EOF__:

```shell
curl <<-EOF -fsSL -H "content-type: text/xml" -d @- http://localhost:8080/ws \
  > target/response.xml && xmllint --format target/response.xml

<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/"
                                  xmlns:gs="http://spring.io/guides/gs-producing-web-service">
   <soapenv:Header/>
   <soapenv:Body>
      <gs:getCountryRequest>
         <gs:name>Spain</gs:name>
      </gs:getCountryRequest>
   </soapenv:Body>
</soapenv:Envelope>

EOF
```

## Инструкция по развертыванию (Деплой)

```bash
# Сохранить образ в Linux архив
docker save soap_image -o soap_image.tar

# Загрузить образ в Docker
docker load -i soap_image.tar

# Запустить контейнер на основе докер образа
docker run --name soap_container -p 80:8080 -d soap_image
```
