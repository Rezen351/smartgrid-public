# Kontrak Data Sensor & Aktuator EMS-RTU

Tabel lengkap metrik data sensor (telemetri) dan aktuator (command) dari keenam simulator plant (SIM-01 hingga SIM-06) berdasarkan dokumen teknis purwarupa EMS-RTU.

| No. | Simulator Plant | Kategori | Nama Parameter / Tag | Unit / Range / State | Catatan Kontrol / Fungsi |
| --- | --- | --- | --- | --- | --- |
| 1 | SIM-01 (Genset) | Sensor (Telemetri) | `state` / `GEN_STATE` | Enum: OFF, CRANKING, WARMUP, RUNNING, COOLDOWN, FAULT | Status operasional genset. |
| 2 | SIM-01 (Genset) | Sensor (Telemetri) | `kW` / `GEN_P_KW` | kW | Actual simulatif daya aktif genset. |
| 3 | SIM-01 (Genset) | Sensor (Telemetri) | `fuel-equivalent L/h` / `GEN_FUEL_LPH` | L/h equivalent | KPI simulatif konsumsi bahan bakar. |
| 4 | SIM-01 (Genset) | Sensor (Telemetri) | `warm-up` / `fault` | BOOL / State | Status pemanasan dan indikasi gangguan. |
| 5 | SIM-01 (Genset) | Aktuator (Command) | `start/stop` / `GEN_CMD_STARTSTOP` | BOOL pulse | Command time-bound; correlation/expiry wajib (pulsa start/stop). |
| 6 | SIM-02 (BESS) | Sensor (Telemetri) | `SOC` / `BESS_SOC_PCT` | % | Hard safety guard untuk kapasitas baterai. |
| 7 | SIM-02 (BESS) | Sensor (Telemetri) | `P` / `BESS_P_KW` | kW | Charge negatif / discharge positif secara konsisten. |
| 8 | SIM-02 (BESS) | Sensor (Telemetri) | `alarm` / `BESS_ALARM` | NONE / LIMITED / FAULT | Memblokir dispatch tidak aman. |
| 9 | SIM-02 (BESS) | Sensor (Telemetri) | `SOH`, `V/I/T`, `power limit` | Persentase / V / A / °C / kW | Kesehatan, parameter elektrik, dan batasan daya. |
| 10 | SIM-02 (BESS) | Aktuator (Command) | virtual charge/discharge response | Setpoint / Command | Respons daya/pengisian baterai. |
| 11 | SIM-03 (Load) | Sensor (Telemetri) | `base load` / `LOAD_BASE_KW` | kW | Baseline load profile. |
| 12 | SIM-03 (Load) | Sensor (Telemetri) | `critical load` / `LOAD_CRITICAL_KW` | kW | Beban kritis yang tidak boleh dimatikan. |
| 13 | SIM-03 (Load) | Sensor (Telemetri) | `flexible load` / `LOAD_FLEX_KW` | kW | Config-limited flexible response. |
| 14 | SIM-03 (Load) | Sensor (Telemetri) | `ENS` / `LOAD_ENS_KW_KWH` | kWh | Energy Not Served (akumulasi KPI). |
| 15 | SIM-03 (Load) | Aktuator (Command) | `flex schedule` / `shed test` | Trigger / Schedule | Uji pelepasan beban / penjadwalan fleksibel. |
| 16 | SIM-04 (Weather) | Sensor (Telemetri) | `GHI` / `GHI_WM2` | W/m² | Global Horizontal Irradiation (Weather replay/profile). |
| 17 | SIM-04 (Weather) | Sensor (Telemetri) | `temperature` / `TEMP_C` | degC | Scenario parameter suhu lingkungan. |
| 18 | SIM-04 (Weather) | Sensor (Telemetri) | `cloud` / `CLOUD_COVER_PCT` | % | Tutupan awan untuk skenario cuaca. |
| 19 | SIM-04 (Weather) | Sensor (Telemetri) | `confidence` / `WEATHER_CONF` | HIGH / MEDIUM / LOW / STALE | Forecast fallback trigger. |
| 20 | SIM-04 (Weather) | Aktuator (Command) | scenario/replay selection | Config / Select | Pemilihan profil cuaca atau reka ulang skenario. |
| 21 | SIM-05 (PV Array) | Sensor (Telemetri) | `P available` / `PV_P_AVAIL_KW` | kW | Derived availability (daya potensial). |
| 22 | SIM-05 (PV Array) | Sensor (Telemetri) | `P_AC` / `PV_P_AC_KW` | kW | Simulated actual output AC. |
| 23 | SIM-05 (PV Array) | Sensor (Telemetri) | `inverter state` / `curtailment` | Enum / Status | Status inverter dan status pembatasan. |
| 24 | SIM-05 (PV Array) | Aktuator (Command) | `PV limit` / `PV_LIMIT_KW` | kW | Virtual curtailment limit (pembatasan daya surya). |
| 25 | SIM-06 (PCS) | Sensor (Telemetri) | `PCS state` / `PCS_STATE` | READY / ENABLED / LIMITED / FAULT | State guard operasional PCS. |
| 26 | SIM-06 (PCS) | Sensor (Telemetri) | `P actual` / `PCS_PACT_KW` | kW | Actual response / reconciliation daya aktif PCS. |
| 27 | SIM-06 (PCS) | Sensor (Telemetri) | `DC/AC status`, `availability`, `efficiency`, `fault` | Status / % / Enum | Kesehatan dan performa sistem konversi daya. |
| 28 | SIM-06 (PCS) | Aktuator (Command) | `PCS setpoint` / `PCS_CMD_P_KW` | kW | Virtual setpoint; safety-clipped (dibatasi pengaman). |
| 29 | SIM-06 (PCS) | Aktuator (Command) | `enable / mode / power setpoint` | Command / Mode | Perintah mode operasi dan pengaktifan PCS. |
