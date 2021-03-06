---
title: IoT Hub Device Provisioning Service — koncepcje dotyczące aprowizacji
description: Ten artykuł zawiera omówienie pojęć dotyczących etapów samodzielnej aprowizacji urządzeń przy użyciu usługi IoT Device Provisioning Service (DPS), IoT Hub i zestawów SDK klienta.
author: wesmc7777
ms.author: wesmc
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: 4755d3ac30a4f6fdc0568dd88fa0e362d7d140a9
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87066564"
---
# <a name="auto-provisioning-concepts"></a>Koncepcje autoaprowizacji

Zgodnie z opisem w temacie [Omówienie](about-iot-dps.md)usługa Device Provisioning to usługa pomocnika, która umożliwia wstępne Inicjowanie obsługi urządzeń w centrum IoT Hub, bez konieczności interwencji człowieka. Po pomyślnej aprowizacji urządzenia łączą się bezpośrednio z wydzielonymi IoT Hub. Ten proces jest określany mianem automatycznej obsługi administracyjnej i zapewnia wbudowaną obsługę rejestracji i początkowej konfiguracji dla urządzeń.

## <a name="overview"></a>Omówienie

Funkcję autoaprowizacji usługi Azure IoT można podzielić na trzy fazy:

1. **Konfiguracja usługi** — jednokrotna konfiguracja wystąpień IoT Hub i IoT Hub Device Provisioning Service platformy Azure, ustanawiająca je i tworzącą powiązania między nimi.

   > [!NOTE]
   > Bez względu na rozmiar rozwiązania IoT, nawet jeśli planujesz obsługę milionów urządzeń, jest to **jednorazowa konfiguracja**.

2. **Rejestrowanie urządzeń** — proces udostępniania wystąpienia usługi Device Provisioning dla urządzeń, które będą próbować zarejestrować się w przyszłości. [Rejestracja](concepts-service.md#enrollment) jest realizowana przez skonfigurowanie informacji o tożsamości urządzenia w usłudze aprowizacji jako "indywidualnej rejestracji" dla jednego urządzenia lub "rejestracji grupowej" dla wielu urządzeń. Tożsamość jest oparta na [mechanizmie zaświadczania](concepts-security.md#attestation-mechanism) , który jest przeznaczony do użytku na urządzeniu, co umożliwia usłudze aprowizacji potwierdzenie autentyczności urządzenia podczas rejestracji:

   - **Moduł TPM**: skonfigurowany jako "indywidualna Rejestracja", tożsamość urządzenia zależy od identyfikatora rejestracji modułu TPM i publicznego klucza poręczenia. Mając na uwadze, że moduł TPM jest [specyfikacją](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/), usługa oczekuje na zaświadczanie zgodnie ze specyfikacją niezależnie od implementacji modułu TPM (sprzętu lub oprogramowania). Zobacz [Inicjowanie obsługi urządzeń: zaświadczanie tożsamości z modułem TPM](https://azure.microsoft.com/blog/device-provisioning-identity-attestation-with-tpm/) , aby uzyskać szczegółowe informacje na temat zaświadczania opartego na module TPM. 

   - **X509**: skonfigurowany jako "indywidualna Rejestracja" lub "Rejestracja grupy", tożsamość urządzenia jest oparta na certyfikacie cyfrowym X. 509, który jest przekazywany do rejestracji jako plik PEM lub CER.

   > [!IMPORTANT]  
   > Chociaż nie jest to wymaganie wstępne w przypadku korzystania z usług Device Provisioning, zdecydowanie zalecamy, aby urządzenie korzystało z sprzętowego modułu zabezpieczeń (HSM) do przechowywania informacji o tożsamości urządzenia poufnego, takich jak klucze i certyfikaty X. 509.

3. **Rejestracja i konfiguracja urządzenia** — zainicjowano po rozpoczęciu przez oprogramowanie rejestracji, które jest tworzone przy użyciu zestawu SDK klienta usługi Device Provisioning odpowiedniego dla mechanizmu urządzenia i zaświadczania. Oprogramowanie nawiąże połączenie z usługą aprowizacji w celu uwierzytelnienia urządzenia i kolejnej rejestracji w IoT Hub. Po pomyślnej rejestracji urządzenie jest dostarczane z IoT Hub unikatowy identyfikator urządzenia i informacje o połączeniu, co pozwala na ściąganie konfiguracji początkowej i rozpoczęcie procesu telemetrii. W środowiskach produkcyjnych ta faza może wystąpić w tygodniach lub miesiącach od poprzednich dwóch faz.

## <a name="roles-and-operations"></a>Role i operacje

Fazy omówione w poprzedniej sekcji mogą obejmować tygodnie lub miesiące, ze względu na rzeczywiste procesy produkcji, takie jak czas produkcji, wysyłka, proces celny itp. Ponadto mogą obejmować działania w wielu rolach, w których występują różne jednostki. W tej sekcji zawarto szczegółowe zapoznaj się z różnymi rolami i operacjami związanymi z każdą fazą, a następnie ilustruje przepływ w diagramie sekwencji. 

Funkcja autoaprowizacji umieszcza także wymagania dotyczące producenta urządzenia, specyficzne dla włączenia mechanizmu zaświadczania. Operacje produkcyjne mogą również odbywać się niezależnie od chronometrażu etapów autouzupełniania, szczególnie w przypadku, gdy nowe urządzenia są nabywane po ustanowieniu autoaprowizacji.

Seria przewodników Szybki Start znajduje się w spisie treści po lewej stronie, aby pomóc w wyjaśnieniu samoobsługowego udostępniania przy użyciu praktycznego środowiska. Aby ułatwić/uprościć proces uczenia, oprogramowanie jest używane do symulowania urządzenia fizycznego na potrzeby rejestracji i rejestracji. Niektóre Przewodniki Szybki Start wymagają wykonania operacji dla wielu ról, w tym operacji dla nieistniejących ról, ze względu na symulowany charakter przewodników Szybki Start.

| Rola | Operacja | Opis |
|------| --------- | ------------|
| Producent | Koduj tożsamość i adres URL rejestracji | Na podstawie używanego mechanizmu zaświadczania producent jest odpowiedzialny za kodowanie informacji o tożsamości urządzenia i adres URL rejestracji usługi Device Provisioning.<br><br>**Przewodniki Szybki Start**: od momentu symulowania urządzenia nie ma roli producenta. Zapoznaj się z rolą dewelopera, aby uzyskać szczegółowe informacje na temat sposobu uzyskiwania tych informacji, które są używane do kodowania przykładowej aplikacji do rejestracji. |
| | Podaj tożsamość urządzenia | Jako podmiot nadawczy informacji o tożsamości urządzenia producent jest odpowiedzialny za komunikowanie się z operatorem (lub wskazanym agentem) lub bezpośrednio z rejestracją w usłudze Device Provisioning za pośrednictwem interfejsów API.<br><br>**Przewodniki Szybki Start**: od momentu symulowania urządzenia nie ma roli producenta. Zobacz rolę operatora, aby uzyskać szczegółowe informacje na temat sposobu uzyskiwania tożsamości urządzenia, która jest używana do rejestrowania symulowanego urządzenia w wystąpieniu usługi Device Provisioning. |
| Operator | Konfigurowanie autoaprowizacji | Ta operacja odpowiada pierwszej fazie inicjowania obsługi.<br><br>**Przewodniki Szybki Start**: wykonywanie roli operatora, Konfigurowanie usługi Device Provisioning i wystąpień IoT Hub w ramach subskrypcji platformy Azure. |
|  | Rejestrowanie tożsamości urządzenia | Ta operacja odpowiada drugiej fazie aprowizacji samoobsługowego.<br><br>**Przewodniki Szybki Start**: wykonujesz rolę operatora, rejestrując urządzenie symulowane w wystąpieniu usługi Device Provisioning. Tożsamość urządzenia jest określana przez metodę zaświadczania symulowaną w przewodniku szybki start (TPM lub X. 509). Szczegółowe informacje o zaświadczeniu można znaleźć w roli dewelopera. |
| Usługa Device Provisioning,<br>IoT Hub | \<all operations\> | W przypadku wdrożenia produkcyjnego z urządzeniami fizycznymi i przewodników szybki start z symulowanymi urządzeniami te role są spełnione za pośrednictwem usług IoT skonfigurowanych w ramach subskrypcji platformy Azure. Role/operacje działają dokładnie tak samo, co usługi IoT różnią się w zależności od aprowizacji urządzeń fizycznych a symulowanych. |
| Deweloper | Kompiluj/Wdróż oprogramowanie rejestracyjne | Ta operacja odpowiada trzeciej fazie inicjowania obsługi. Deweloper jest odpowiedzialny za tworzenie i wdrażanie oprogramowania do rejestracji na urządzeniu przy użyciu odpowiedniego zestawu SDK.<br><br>**Przewodniki Szybki Start**: kompilacja przykładowej aplikacji do rejestracji symuluje rzeczywiste urządzenie dla wybranej platformy/języka, który działa na stacji roboczej (zamiast wdrażać ją na urządzeniu fizycznym). Aplikacja do rejestracji wykonuje te same operacje, które zostały wdrożone na urządzeniu fizycznym. Należy określić metodę zaświadczania (Certyfikat modułu TPM lub X. 509) oraz adres URL rejestracji i "zakres identyfikatorów" wystąpienia usługi Device Provisioning Service. Tożsamość urządzenia jest określana przez logikę zaświadczania zestawu SDK w czasie wykonywania na podstawie określonej metody: <ul><li>**Zaświadczanie modułu TPM** — na stacji roboczej deweloperskiej jest uruchomiona [aplikacja symulatora modułu TPM](how-to-use-sdk-tools.md#trusted-platform-module-tpm-simulator). Po uruchomieniu oddzielna aplikacja jest używana do wyodrębnienia "klucza poręczenia" i "Identyfikator rejestracji" do użycia podczas rejestrowania tożsamości urządzenia. Logika zaświadczania zestawu SDK używa również symulatora podczas rejestracji, aby przedstawić podpisany token sygnatury dostępu współdzielonego w celu uwierzytelniania i weryfikacji rejestracji.</li><li>**Zaświadczanie** [o certyfikatach x509 — wygenerowanie certyfikatu](how-to-use-sdk-tools.md#x509-certificate-generator)jest możliwe za pomocą narzędzia. Po wygenerowaniu należy utworzyć plik certyfikatu wymagany do użycia podczas rejestracji. Logika zaświadczania zestawu SDK używa również certyfikatu podczas rejestracji, aby można było zaprezentować uwierzytelnianie i weryfikację rejestracji.</li></ul> |
| Urządzenie | Rozruchu i zarejestruj | Ta operacja jest zgodna z trzecią fazą samoobsługowego inicjowania obsługi przez oprogramowanie do rejestracji urządzeń utworzone przez dewelopera. Aby uzyskać szczegółowe informacje, zobacz rolę dewelopera. Po pierwszym rozruchu: <ol><li>Aplikacja nawiązuje połączenie z wystąpieniem usługi Device Provisioning, używając globalnego adresu URL i usługi "Scope ID" określonego podczas tworzenia.</li><li>Po nawiązaniu połączenia urządzenie jest uwierzytelniane w oparciu o metodę zaświadczania i tożsamość określoną podczas rejestracji.</li><li>Po uwierzytelnieniu urządzenie zostanie zarejestrowane przy użyciu wystąpienia IoT Hub określonego przez wystąpienie usługi aprowizacji.</li><li>Po pomyślnej rejestracji unikatowy identyfikator urządzenia i punkt końcowy IoT Hub są zwracane do aplikacji rejestracji w celu komunikowania się z IoT Hub.</li><li> Z tego miejsca urządzenie może pobrać stan wstępnej [splotu urządzenia](~/articles/iot-hub/iot-hub-devguide-device-twins.md) w celu skonfigurowania i rozpocząć proces raportowania danych telemetrycznych.</li></ol>**Przewodniki Szybki Start**: ponieważ urządzenie jest symulowane, oprogramowanie do rejestracji działa na stacji roboczej deweloperskiej.|

Poniższy diagram podsumowuje role i sekwencjonowanie operacji podczas samoobsługowego dostarczania urządzeń:
<br><br>
[![Sekwencja autoaprowizacji dla urządzenia](./media/concepts-auto-provisioning/sequence-auto-provision-device-vs.png)](./media/concepts-auto-provisioning/sequence-auto-provision-device-vs.png#lightbox) 

> [!NOTE]
> Opcjonalnie producent może również wykonać operację "Zarejestruj tożsamość urządzenia" przy użyciu interfejsów API usługi Device Provisioning (zamiast za pośrednictwem operatora). Aby uzyskać szczegółową dyskusję na temat tej sekwencji i innych informacji, zobacz temat [Rejestracja urządzenia dotykowego w usłudze Azure IoT wideo](https://youtu.be/cSbDRNg72cU?t=2460) (od znacznika 41:00).

## <a name="roles-and-azure-accounts"></a>Role i konta platformy Azure

Sposób, w jaki każda rola jest mapowana na konto platformy Azure, jest zależna od scenariusza i istnieje kilka scenariuszy, które mogą być używane. Typowe wzorce poniżej powinny pomóc w ustaleniu ogólnego zagadnienia dotyczącego sposobu, w jaki role są zwykle mapowane na konto platformy Azure.

#### <a name="chip-manufacturer-provides-security-services"></a>Producent mikroukładu dostarcza usługi zabezpieczeń

W tym scenariuszu producent zarządza zabezpieczeniami dla klientów poziomu jednego. Ten scenariusz może być preferowany przez ten poziom — jeden klient nie musi zarządzać szczegółowymi zabezpieczeniami. 

Producent wprowadza zabezpieczenia do sprzętowych modułów zabezpieczeń (sprzętowych modułów zabezpieczeń). Zabezpieczenia te mogą obejmować producentom uzyskanie kluczy, certyfikatów itp. od potencjalnych klientów, którzy mają już wystąpienia usługi DPS i konfigurację grup rejestracji. Producent może również generować te informacje o zabezpieczeniach dla swoich klientów.

W tym scenariuszu mogą być uwzględnione dwa konta platformy Azure:

- **#1 konta**: możliwe, że w ramach ról operatora i dewelopera chcesz mieć pewien stopień. Ta strona może zakupić wióry modułu HSM od producenta. Te wióry są wskazywane dla wystąpień DPS skojarzonych z #1 konta. W przypadku rejestracji w usłudze DPS ta strona może dzierżawić urządzenia wielu klientom z wieloma poziomami przez ponowne skonfigurowanie ustawień rejestracji urządzeń w usłudze DPS. Ta strona może również mieć przypisane centra IoT przeznaczone dla systemów zaplecza użytkowników końcowych do interfejsów w programie w celu uzyskania dostępu do telemetrii urządzenia itp. W tym drugim przypadku drugie konto może nie być konieczna.

- **#2 konta**: użytkownicy końcowi, poziom — dwaj klienci mogą mieć własne centra IoT Hub. Strona skojarzona z kontem #1 jedynie punkty dzierżawione urządzenia do właściwego centrum w ramach tego konta. Ta konfiguracja wymaga łączenia usługi DPS i centrów IoT na kontach platformy Azure, które można wykonać za pomocą szablonów Azure Resource Manager.

#### <a name="all-in-one-oem"></a>Wszyscy producenci OEM

Producent może być "Wszyscy producenci OEM", w przypadku którego będzie wymagana tylko jedno konto producenta. Producent obsługuje kompleksowe zabezpieczenia i Inicjowanie obsługi administracyjnej.

Producent może udostępnić aplikacji opartej na chmurze klientom, którzy kupują urządzenia. Ta aplikacja będzie mieć interfejs z IoT Hub przyznanym przez producenta.

W tym scenariuszu maszyny sprzedaży lub zautomatyzowane komputery kawowe reprezentują przykłady.




## <a name="next-steps"></a>Następne kroki

Pomocne może być zakładanie tego artykułu jako punktu odniesienia podczas pracy przez odpowiednie Przewodniki Szybki Start dotyczące samoobsługowego udostępniania. 

Zacznij od wykonania kroku "Konfigurowanie automatycznej aprowizacji" przewodnika Szybki Start, który najlepiej odpowiada preferencjom narzędzia do zarządzania, które przeprowadzi Cię przez fazę "Konfiguracja usługi":

- [Konfigurowanie samoobsługowego inicjowania obsługi przy użyciu interfejsu wiersza polecenia platformy Azure](quick-setup-auto-provision-cli.md)
- [Skonfiguruj funkcję autoaprowizacji przy użyciu Azure Portal](quick-setup-auto-provision.md)
- [Konfigurowanie samoobsługowego inicjowania obsługi przy użyciu szablonu Menedżer zasobów](quick-setup-auto-provision-rm.md)

Następnie przejdź do przewodnika Szybki Start "Autouzupełnianie urządzenia symulowanego", który odpowiada mechanizmowi zaświadczania urządzenia i zestawie SDK/języka usługi aprowizacji urządzeń. W tym przewodniku szybki start przeprowadzisz etapy "rejestracja urządzeń" i "rejestracja urządzeń i konfiguracji": 

| Mechanizm zaświadczania urządzenia symulowanego | Zestaw SDK/język szybkiego startu |
| -------------------------------------- | ----------------------- |
| Moduł TPM | [S](quick-create-simulated-device.md)<br>[Java](quick-create-simulated-device-tpm-java.md)<br>[C#](quick-create-simulated-device-tpm-csharp.md)<br>[Python](quick-create-simulated-device-tpm-python.md) |
| Certyfikat X. 509 | [S](quick-create-simulated-device-x509.md)<br>[Java](quick-create-simulated-device-x509-java.md)<br>[C#](quick-create-simulated-device-x509-csharp.md)<br>[Node.js](quick-create-simulated-device-x509-node.md)<br>[Python](quick-create-simulated-device-x509-python.md) |




