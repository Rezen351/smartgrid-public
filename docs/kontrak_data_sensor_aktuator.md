# Kontrak Data Sensor & Aktuator EMS-RTU

Tabel lengkap metrik data sensor (telemetri) dan aktuator (command) dari keenam simulator plant (SIM-01 hingga SIM-06) berdasarkan dokumen teknis purwarupa EMS-RTU.

| No. | Simulator Plant | Kategori | Nama Parameter / Tag | Display Name | Unit / Range / State | Nama Tabel TimescaleDB | Catatan Kontrol / Fungsi |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | SIM-01 (Genset) | Sensor (Telemetri) | `state` / `GEN_STATE` | Status Genset | Enum: OFF, CRANKING, WARMUP, RUNNING, COOLDOWN, FAULT | `genset_state` | Status operasional genset. |
| 2 | SIM-01 (Genset) | Sensor (Telemetri) | `kW` / `GEN_P_KW` | Daya Aktif Genset | kW | `genset_power` | Actual simulatif daya aktif genset. |
| 3 | SIM-01 (Genset) | Sensor (Telemetri) | `fuel-equivalent L/h` / `GEN_FUEL_LPH` | Konsumsi Bahan Bakar | L/h equivalent | `genset_fuel` | KPI simulatif konsumsi bahan bakar. |
| 4 | SIM-01 (Genset) | Sensor (Telemetri) | `warm-up` / `GEN_WARMUP` | Warm-Up Genset | BOOL | `genset_warmup` | Status pemanasan genset. |
| 5 | SIM-01 (Genset) | Sensor (Telemetri) | `fault` / `GEN_FAULT` | Fault Genset | BOOL / State | `genset_fault` | Indikasi gangguan genset. |
| 6 | SIM-01 (Genset) | Aktuator (Command) | `start/stop` / `GEN_CMD_STARTSTOP` | Start/Stop Genset | BOOL pulse | `genset_cmd` | Command time-bound; correlation/expiry wajib (pulsa start/stop). |
| 7 | SIM-02 (BESS) | Sensor (Telemetri) | `SOC` / `BESS_SOC_PCT` | SOC Baterai | % | `bess_soc` | Hard safety guard untuk kapasitas baterai. |
| 8 | SIM-02 (BESS) | Sensor (Telemetri) | `P` / `BESS_P_KW` | Daya BESS | kW | `bess_power` | Charge negatif / discharge positif secara konsisten. |
| 9 | SIM-02 (BESS) | Sensor (Telemetri) | `alarm` / `BESS_ALARM` | Alarm BESS | NONE / LIMITED / FAULT | `bess_alarm` | Memblokir dispatch tidak aman. |
| 10 | SIM-02 (BESS) | Sensor (Telemetri) | `SOH` / `BESS_SOH_PCT` | SOH Baterai | % | `bess_soh` | Kesehatan baterai. |
| 11 | SIM-02 (BESS) | Sensor (Telemetri) | `V` / `BESS_V` | Tegangan Baterai | V | `bess_v` | Tegangan baterai. |
| 12 | SIM-02 (BESS) | Sensor (Telemetri) | `I` / `BESS_I` | Arus Baterai | A | `bess_i` | Arus baterai. |
| 13 | SIM-02 (BESS) | Sensor (Telemetri) | `T` / `BESS_T` | Suhu Baterai | °C | `bess_t` | Suhu baterai. |
| 14 | SIM-02 (BESS) | Sensor (Telemetri) | `power limit` / `BESS_P_LIMIT_KW` | Batas Daya BESS | kW | `bess_p_limit` | Batasan daya pengisian/pembuangan. |
| 15 | SIM-02 (BESS) | Aktuator (Command) | virtual charge/discharge response | Respons Daya BESS | Setpoint / Command | `bess_cmd` | Respons daya/pengisian baterai. |
| 16 | SIM-03 (Load) | Sensor (Telemetri) | `base load` / `LOAD_BASE_KW` | Beban Dasar | kW | `load_base_kw` | Baseline load profile. |
| 17 | SIM-03 (Load) | Sensor (Telemetri) | `flexible load` / `LOAD_FLEX_KW` | Beban Fleksibel | kW | `load_flex_kw` | Config-limited flexible response. |
| 18 | SIM-03 (Load) | Sensor (Telemetri) | `critical load` / `LOAD_CRITICAL_KW` | Beban Kritis | kW | `load_critical_kw` | Beban kritis yang tidak boleh dimatikan. |
| 19 | SIM-03 (Load) | Sensor (Telemetri) | `ENS` / `LOAD_ENS_KWH` | Energy Not Served | kWh | `load_ens` | Energy Not Served (akumulasi KPI). |
| 20 | SIM-03 (Load) | Aktuator (Command) | `flex schedule` / `shed test` | Jadwal Fleksibel / Shed Test | Trigger / Schedule | `load_cmd` | Uji pelepasan beban / penjadwalan fleksibel. |
| 21 | SIM-04 (Weather) | Sensor (Telemetri) | `GHI` / `GHI_WM2` | Iradiasi Horisontal Global | W/m² | `ghi_wm2` | Global Horizontal Irradiation (Weather replay/profile). |
| 22 | SIM-04 (Weather) | Sensor (Telemetri) | `temperature` / `TEMP_C` | Suhu Lingkungan | degC | `temp_c` | Scenario parameter suhu lingkungan. |
| 23 | SIM-04 (Weather) | Sensor (Telemetri) | `cloud` / `CLOUD_COVER_PCT` | Tutupan Awan | % | `cloud_cover_pct` | Tutupan awan untuk skenario cuaca. |
| 24 | SIM-04 (Weather) | Sensor (Telemetri) | `confidence` / `WEATHER_CONF` | Confidence Cuaca | HIGH / MEDIUM / LOW / STALE | `weather_conf` | Forecast fallback trigger. |
| 25 | SIM-04 (Weather) | Aktuator (Command) | scenario/replay selection | Skenario Cuaca | Config / Select | `weather_cmd` | Pemilihan profil cuaca atau reka ulang skenario. |
| 26 | SIM-05 (PV Array) | Sensor (Telemetri) | `P available` / `PV_P_AVAIL_KW` | Daya Tersedia PV | kW | `pv_p_avail_kw` | Derived availability (daya potensial). |
| 27 | SIM-05 (PV Array) | Sensor (Telemetri) | `P_AC` / `PV_P_AC_KW` | Daya AC PV | kW | `pv_p_ac_kw` | Simulated actual output AC. |
| 28 | SIM-05 (PV Array) | Sensor (Telemetri) | `inverter state` / `PV_INVERTER_STATE` | Status Inverter | Enum | `pv_inverter_state` | Status inverter. |
| 29 | SIM-05 (PV Array) | Sensor (Telemetri) | `curtailment` / `PV_CURTAILMENT_STATUS` | Status Curtailment | Enum / Status | `pv_curtailment` | Status pembatasan daya surya. |
| 30 | SIM-05 (PV Array) | Aktuator (Command) | `PV limit` / `PV_LIMIT_KW` | Batas Daya PV | kW | `pv_cmd` | Virtual curtailment limit (pembatasan daya surya). |
| 31 | SIM-06 (PCS) | Sensor (Telemetri) | `PCS state` / `PCS_STATE` | Status PCS | READY / ENABLED / LIMITED / FAULT | `pcs_state` | State guard operasional PCS. |
| 32 | SIM-06 (PCS) | Sensor (Telemetri) | `P command` / `PCS_CMD_P_KW` | Setpoint Daya PCS | kW | `pcs_set_power` | Setpoint daya dari EMS. |
| 33 | SIM-06 (PCS) | Sensor (Telemetri) | `P actual` / `PCS_P_ACT_KW` | Daya Aktual PCS | kW | `pcs_actual` | Actual response / reconciliation daya aktif PCS. |
| 34 | SIM-06 (PCS) | Sensor (Telemetri) | `DC/AC status` / `PCS_DC_AC_STATUS` | Status DC/AC PCS | Enum / Status | `pcs_dc_ac_status` | Status konversi DC/AC. |
| 35 | SIM-06 (PCS) | Sensor (Telemetri) | `availability` / `PCS_AVAILABILITY_PCT` | Ketersediaan PCS | % | `pcs_availability` | Ketersediaan PCS. |
| 36 | SIM-06 (PCS) | Sensor (Telemetri) | `efficiency` / `PCS_EFFICIENCY_PCT` | Efisiensi PCS | % | `pcs_efficiency` | Efisiensi sistem konversi daya. |
| 37 | SIM-06 (PCS) | Sensor (Telemetri) | `fault` / `PCS_FAULT` | Fault PCS | BOOL / State | `pcs_fault` | Indikasi gangguan PCS. |
| 38 | SIM-06 (PCS) | Aktuator (Command) | `enable / mode / power setpoint` | Operasi PCS | Command / Mode | `pcs_op_cmd` | Perintah mode operasi dan pengaktifan PCS. |
