---
{"dg-publish":true,"permalink":"/fashaun-dev-world/secureboot-and-tpm/","noteIcon":""}
---


## Secure Boot 與 TPM 基礎

- **UEFI Secure Boot (安全啟動)**
    - Secure Boot 是 UEFI 韌體標準的一部分，其主要目標是在開機過程中驗證各個組件（如作業系統載入器、驅動程式）的數位簽章。它確保只有經過授權且未被竄改的軟體才能在開機時執行，從而防止惡意軟體（如 Rootkits 或 Bootkits）在作業系統載munderover之前載入。
    - **運作原理**: Secure Boot 依賴一組金鑰資料庫來運作：
        - **平台金鑰 (Platform Key, PK)**: 由硬體製造商(OEM)安裝，代表平台擁有者與韌體之間的信任關係。
        - **金鑰交換金鑰 (Key Exchange Key, KEK)**: 建立韌體與作業系統之間的信任。用於授權對簽章資料庫的修改。
        - **簽章資料庫 (Signature Database, db)**: 包含受信任軟體（如作業系統載入器、驅動程式）的公鑰或雜湊值。
        - **撤銷簽章資料庫 (Revoked Signatures Database, dbx)**: 包含已知惡意或已被洩漏金鑰的簽章，用於阻擋這些軟體執行。
    - **出處**: [Trenton Systems - What is Secure Boot?](https://www.trentonsystems.com/en-us/resource-hub/blog/what-is-secure-boot)

- **Trusted Platform Module (TPM, 可信平台模組)**
    - TPM 是一個國際標準，其晶片是一個安全的加密處理器，旨在提供基於硬體的安全性相關功能。它透過物理安全機制來防禦惡意軟體和物理竄改。
    - **核心功能**:
        - **金鑰的生成、儲存與使用限制**: TPM 可以在晶片內部生成和儲存加密金鑰，並設定這些金鑰的使用條件，使其無法被輕易匯出或盜用。
        - **平台完整性度量 (Integrity Measurement)**: 在開機過程中，TPM 會對載入的程式碼（包含韌體、驅動程式、作業系統核心）進行雜湊運算，並將這些「度量值」安全地儲存在平台設定暫存器 (PCRs) 中。
        - **平台驗證 (Attestation)**: TPM 可以產生一份報告，證明系統開機過程中的狀態（即 PCRs 中的度量值），並用其內建的唯一金鑰（Endorsement Key, EK）對報告簽章，讓遠端伺服器可以驗證該電腦的健康狀態是否值得信賴。
    - **出處**: [Microsoft - Trusted Platform Module Technology Overview](https://learn.microsoft.com/en-us/windows/security/hardware-security/tpm/trusted-platform-module-overview)


## Secure Boot 與 TPM 的協同運作與進階應用

Secure Boot 和 TPM 雖然是兩項獨立的技術，但它們協同工作時能提供更強大的系統安全性，建立一個從硬體到作業系統的「信任根」(Root of Trust)。

- **開機過程中的協同運作**:
    1.  **開機啟動**: 電腦通電後，UEFI 韌體開始執行。
    2.  **Secure Boot 驗證**: UEFI 韌體使用 `db` 資料庫中的公鑰來驗證下一個要執行的軟體（例如作業系統載入器）的數位簽章。如果驗證通過，且簽章不在 `dbx` 黑名單中，則允許其執行。
    3.  **TPM 度量**: 與此同時，每當一個經過驗證的組件被載入時，TPM 都會測量其雜湊值並儲存到 PCRs 中。這個過程從 UEFI 韌體自身開始，一路延伸到作業系統載入器和核心。
    - 簡單來說，**Secure Boot 扮演著「守門員」的角色**，確保只有「對的人」（擁有合法簽章的軟體）才能進入。而 **TPM 則扮演著「公證人」的角色**，忠實記錄「是誰」以及「以什麼順序」進入，並為整個過程的完整性提供證明。

- **進階應用 - BitLocker 磁碟加密**:
    - BitLocker 是 Windows 的全磁碟加密功能，它利用 TPM 來保護加密金鑰。
    - **金鑰密封 (Sealing)**: BitLocker 會將解鎖磁碟的金鑰「密封」到 TPM 中，並綁定到特定的 PCR 度量值。
    - **解鎖**: 當系統下次開機時，TPM 會再次測量開機過程。只有當測量值與密封時完全一致，TPM 才會「解封」並釋放金鑰，讓作業系統能夠解密磁碟並成功啟動。
    - **保護機制**: 如果攻擊者試圖竄改開機過程（例如，植入一個惡意的 Bootloader），TPM 測量到的 PCR 值將會不同，TPM 會拒絕解封金鑰，從而保護磁碟上的資料不被竊取。

- **進階應用 - 設備健康狀況證明 (Device Health Attestation)**:
    - 企業可以利用 TPM 的 Attestation 功能來驗證員工裝置的安全性。MDM (行動裝置管理) 伺服器可以要求裝置提供一份由 TPM 簽署的健康報告。
    - 這份報告可以證明該裝置是否啟用了 Secure Boot、BitLocker，以及開機過程是否安全。只有通過驗證的裝置才能被允許存取公司的敏感資源。

## 總結

Secure Boot 和 TPM 是現代電腦安全的基石。

- **Secure Boot** 透過數位簽章驗證，確保在開機過程中只執行受信任的軟體，從源頭上阻擋了惡意程式的滲透。
- **TPM** 提供了一個基於硬體的信任根，用於安全地儲存金鑰、度量系統開機過程的完整性，並向外界證明系統的健康狀態。

兩者結合，為作業系統提供了一個可信的執行環境，是實現全碟加密、設備存取控制等進階安全功能的基礎。

## 問題與未來

- 金鑰管理的複雜性：如何安全地更新 `db` 和 `dbx` 資料庫以應對新的威脅或修補漏洞？如果 OEM 的金鑰洩漏會發生什麼事？
- 信任鏈的延伸：目前的機制主要保護 CPU 和主機板韌體，但其他硬體元件（如網卡、顯卡、硬碟）的韌體是否也需要同樣的保護機制？（這也催生了如 Google Titan, Apple T2 等客製化安全晶片的發展）。
- 開源韌體與限制：Intel Boot Guard 等技術雖然增強了韌體驗證，但也限制了使用者安裝如 Coreboot 等開源韌體的自由度，這在安全與自由之間產生了權衡。

## 標籤

#Security #Boot #UEFI #TPM #Hardware #Firmware

## 參考資料

- [Microsoft - Trusted Platform Module Technology Overview](https://learn.microsoft.com/en-us/windows/security/hardware-security/tpm/trusted-platform-module-overview)
- [Trenton Systems - What is Secure Boot?](https://www.trentonsystems.com/en-us/resource-hub/blog/what-is-secure-boot)
- [Communications of the ACM - Securing the Boot Process](https://cacm.acm.org/practice/securing-the-boot-process/)

## 工業級 SoC 的 Secure Boot 實踐

工業級或嵌入式系統的 Secure Boot 流程與 PC BIOS/UEFI 的機制有所不同。它不依賴於一個標準化的 UEFI 介面，而是由 SoC 晶片製造商定義，從晶片內部一個不可變的 Boot ROM 開始，建立硬體信任根 (Hardware Root of Trust)。這個信任鏈會一步步驗證接下來載入的軟體（如 Bootloader、Kernel），確保整個開機過程的完整性與真實性。

---

### TI Sitara AM335x Secure Boot

TI AM335x 系列的安全啟動功能是作為一個「可選配」的項目提供，通常需要與 TI 進行商務合作並簽署 NDA 才能取得完整的客製化金鑰燒錄工具與文件。然而，其安全啟動的核心概念是公開的。

-   **硬體信任根 (Hardware Root of Trust)**
    -   **Boot ROM**: AM335x 內部有一個原廠燒錄的 Boot ROM。當晶片上電或重置時，這是第一個執行的程式碼。
    -   **金鑰燒錄**: 開發者需產生一組非對稱金鑰（公鑰/私鑰）。其中，**公鑰的雜湊值 (Hash)** 會被永久性地燒錄 (fuse) 到晶片的 eFuses (一次性可程式化熔絲) 中。這個動作是不可逆的，一旦燒錄，該晶片就與這組公鑰綁定。這個燒錄的公鑰雜湊值，就是硬體信任根的核心。
    -   **加密與簽章工具**: TI 提供（在NDA下）工具讓開發者可以使用對應的私鑰來簽署 Bootloader (如 U-Boot 的 SPL)。

-   **Bootloader (U-Boot) 的安全啟動流程**
    1.  **第一階段驗證 (SPL)**:
        -   Boot ROM 從指定的啟動裝置（如 eMMC, SD Card, SPI Flash）載入第一階段的 Bootloader (SPL - Secondary Program Loader)。
        -   在載入後，Boot ROM 會使用晶片內部的硬體加密引擎，計算 SPL 映像檔的雜湊值，並用燒錄在 eFuses 中的公鑰雜湊值來驗證 SPL 的數位簽章。
        -   只有在簽章驗證通過後，Boot ROM 才會跳轉執行 SPL 的程式碼。如果驗證失敗，啟動過程將會被中斷。
    2.  **第二階段驗證 (U-Boot)**:
        -   SPL 被成功驗證並執行後，信任鏈就傳遞到了 SPL。
        -   接著，SPL 會負責載入完整的 U-Boot 映像檔。
        -   SPL 會使用類似於 Boot ROM 的驗證機制（通常是透過軟體實現的加密函式庫）來驗證 U-Boot 主映像檔的簽章。
        -   驗證通過後，SPL 才會跳轉執行 U-Boot。

-   **Linux Kernel 的安全啟動流程**
    1.  **信任鏈延伸**: 信任鏈此時已經傳遞到 U-Boot。
    2.  **映像檔驗證**: U-Boot 會載入 Linux 核心映像檔（通常是 `uImage`, `zImage` 或是包含核心與 DTB 的 `FIT image`）。
    3.  **簽章確認**: 在啟動核心之前，U-Boot 會使用其內建的公鑰（這個公鑰是在編譯 U-Boot 時就已打包進去，而整個 U-Boot 又被 SPL 驗證過）來驗證核心映像檔的數位簽章。
    4.  **啟動核心**: 只有在核心映像檔的簽章被成功驗證後，U-Boot 才會將執行權交給 Linux 核心，完成整個安全啟動鏈。

-   **關於 TPM**:
    -   TI AM335x 本身**並未整合**標準的 TPM 模組。然而，在需要符合 TPM 規範的應用中，開發者可以透過 I2C 或 SPI 介面外掛一顆獨立的 TPM 安全晶片，並在 U-Boot 和 Linux Kernel 中載入相應的驅動程式來實現 TPM 的功能，例如金鑰儲存、平台完整性度量 (PCRs) 等。

-   **參考資料**:
    -   [TI White Paper: Secure boot on embedded Sitarä™ processors](https://www.ti.com/lit/wp/spry305a/spry305a.pdf)
    -   [TI E2E Forum: Secure boot in Sitara series AM335x](https://e2e.ti.com/support/processors-group/processors/f/processors-forum/1104345/am3352-secure-boot-in-sitara-series-am335x-am3352)

---

### NXP i.MX 8M Mini Secure Boot (HABv4)

NXP i.MX 系列的 SoC 使用一種名為 **High Assurance Boot (HAB)** 的機制來實現安全啟動。在 i.MX 8M Mini 上，使用的是第四版，即 **HABv4**。相較於 TI，NXP 的文件和工具通常更加開放。

-   **硬體信任根 (Hardware Root of Trust)**
    -   **Boot ROM (HAB Library)**: i.MX 8M Mini 的 Boot ROM 中內建了 HAB 函式庫，這是信任鏈的起點。
    -   **eFuse 與 SRK**: 開發者需要產生一組或多組**超級根金鑰 (Super Root Key, SRK)**。SRK 的公鑰經過 SHA-256 運算後的雜湊值 (SRK hash)，會被永久性地燒錄到晶片的 eFuses 中。一旦燒錄並將晶片設定為「安全模式 (Closed state)」，這個過程就不可逆。
    -   **CST (Code Signing Tool)**: NXP 提供 `cst` 這個工具，讓開發者可以產生 SRK 金鑰、建立憑證，並使用私鑰來簽署啟動映像檔。

-   **Bootloader (U-Boot) 的安全啟動流程 (多階段)**
    -   i.MX 8M Mini 的啟動流程比 AM335x 更為複雜，因為它涉及到 ARM TrustZone (ATF - ARM Trusted Firmware) 和 DDR 韌體。整個啟動映像檔 (`flash.bin`) 包含多個組件。
    1.  **第一階段驗證 (SPL + DDR Firmware)**:
        -   Boot ROM 首先會載入 `flash.bin` 的一部分，其中包含 SPL 和 DDR 韌體。
        -   `flash.bin` 內嵌了一個 **Image Vector Table (IVT)**，它指向 **Command Sequence File (CSF)** 的位置。CSF 是一個二進位的資料結構，它告訴 HAB 如何驗證映像檔。
        -   HAB 會根據 CSF 的指令，使用燒錄在 eFuses 中的 SRK 公鑰雜湊值來驗證 SPL 和 DDR 韌體的簽章。
        -   如果驗證成功，Boot ROM 才會執行 SPL。
    2.  **第二階段驗證 (FIT Image: U-Boot, ATF, OP-TEE)**:
        -   SPL 執行後，會初始化 DDR 記憶體。
        -   接著，SPL 會使用 HAB 提供的 API，將信任鏈延伸，以驗證 `flash.bin` 中剩餘的部分，這部分通常打包成一個 **FIT (Flattened Image Tree) image**。
        -   這個 FIT image 包含了完整的 U-Boot、ARM TrustZone 韌體 (ATF/`bl31.bin`)，以及可選的 OP-TEE 安全作業系統。
        -   SPL 會呼叫 HAB API，使用相同的 SRK 金鑰來驗證整個 FIT image 的簽章。
        -   驗證成功後，SPL 會先跳轉到 ATF，再由 ATF 將執行權交給 U-Boot。

-   **Linux Kernel 的安全啟動流程**
    1.  **信任鏈延伸至 U-Boot**: 經過前述兩個階段的驗證，U-Boot 已經是一個受信任的軟體。
    2.  **FIT Image 驗證**: 為了確保一致性與安全性，Linux 核心、設備樹 (DTB) 和 Ramdisk 通常也會被打包成一個簽署過的 FIT image。
    3.  **U-Boot 驗證核心**: 在啟動核心前，U-Boot 會再次呼叫 HAB API (`hab_auth_img` 命令)，使用 SRK 金鑰來驗證這個包含核心的 FIT image 的簽章。
    4.  **啟動核心**: 驗證通過後，U-Boot 才會載入核心並啟動系統。

-   **關於 TPM**:
    -   i.MX 8M Mini **內建了硬體加密加速與保障模組 (CAAM - Cryptographic Acceleration and Assurance Module)**，以及對 **ARM TrustZone** 的支援。
    -   **TrustZone** 可以提供一個安全的執行環境 (TEE - Trusted Execution Environment)，如 OP-TEE，其功能類似於一個韌體 TPM (fTPM)。它可以在一個與主要作業系統隔離的環境中，安全地處理金鑰和敏感資料。
    -   雖然不是一個標準的 TCG TPM 晶片，但透過 CAAM 和 TrustZone/OP-TEE 的組合，i.MX 8M Mini 可以實現許多與 TPM 類似的安全功能，例如安全金鑰儲存、加密運算和硬體隔離。

-   **參考資料**:
    -   [Amarula Solutions: I.MX8MM Secure Boot using High Assurance Boot v4](https://wiki.amarulasolutions.com/opensource/uboot/secure_boot/imx8mm_habv4.html)
    -   [Ezurio: High Assurance Boot (HAB) - i.MX8M edition](https://www.ezurio.com/resources/software-announcements/high-assurance-boot-hab-i-mx8m-edition?utm_medium=referral&utm_campaign=bdwebsite&utm_source=boundarydevices.com)