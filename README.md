<a name="readme-top"></a>

📘 Project Overview <br>
The Real-Time Attendance (TNA) system is designed to automate and streamline employee attendance processing using biometric swipes data pr Mobile Punches. It integrates with our RTA Engine to ensure accurate, rule-based attendance status updates and compliance across various scenarios, such as shifts, late/early punches, OT and backdated corrections(if applicable).

🔧 Core Functionalities
i) Manage Shift Rules
Defines and enforces rules related to shift timings, overtime, night differentials, flexible shifts, and buffer windows (pre/post shift limits).

ii) Track Attendance Metrics
Captures and validates punch-in and punch-out data to determine total working hours, late comings, early goings, and shift adherence.

iii) Calculate Compliance
Applies grace period logic for late arrivals and early departures, factoring in configurable thresholds and allowed occurrences within an attendance cycle.

iv) Update Attendance Records
Maintains updated and accurate attendance details in the Rostering and other core tables, including fields like Present, Half Day, Absent, OT hours, ND hours, and capped attendance timings.

v) Process Backdated Attendance
Automatically recalculates attendance in cases of shift change, regularization, leave approval, or group/department updates to ensure consistency across records.
## 🚀 Cool features of TNA

- 🆕 [**Automated Attendance Processing:Eliminates manual tracking with real-time calculations.**]
- 🆕 [**Overtime Management:Calculates PreOT, PostOT, and other overtime metrics based on shift and day type.**]
- 🆕 [**Grace Period Validation:Tracks allowable instances of late arrivals and early departures.**]


**Real Time Attendance(TNA) Steps:**

- Step1:[ Create and execute the stored procedure TNA.SetExtraTimeRule_Z2 ](https://github.com/zinghrcore/z2-tna-db/blob/master/25.%5BTNA%5D.%5BSetExtraTimeRule_Z2%5D.sql)
- Step2:[Set the database compatibility level to 150.](https://github.com/zinghrcore/z2-tna-db/blob/master/00.COMPATIBILITY_LEVEL)
- Step3:[If not exist,Add DayType Column in Rostering](https://github.com/zinghrcore/z2-tna-db/blob/master/0field%20master%20and%20rostering%20daytype.sql)
- Step4:[Create table TNA.ShiftWithPrePost](https://github.com/zinghrcore/z2-tna-db/blob/master/001.shiftwithprepost.sql)
- Step5:[For FlatTable Population create SP  TNA. Setempshiftjson ](https://github.com/zinghrcore/z2-tna-db/blob/master/26.%20%5BTNA%5D.%5BSetEmpShiftJSONData%5D.sql)
- Step6:[Create stored procedure  TNA.LateComingCalculationOnPunchOut_Z2 ](https://github.com/zinghrcore/z2-tna-db/blob/master/17.%5BTNA%5D.%5BLateComingCalculationOnPunchOut_Z2%5D.sql)
- Step7:[Create stored procedure  TNA.EarlygoingCalculationOnPunchOut_Z2 ](https://github.com/zinghrcore/z2-tna-db/blob/master/19.%5BTNA%5D.%5BEarlyGoingCalculationOnPunchOut_Z2%5D.sql)
- Step8:[Create stored procedure  PunchInOnLogin ](https://github.com/zinghrcore/z2-tna-db/blob/master/16.%5BTNA%5D.%5BPunchInOnLogin%5D.sql)
- Step9:[Create stored procedure   PunchInOnThroughBio_z2 ](https://github.com/zinghrcore/z2-tna-db/blob/master/18.%5BTNA%5D.%5BPunchInOnThroughBio_z2%5D.sql)
- Step10:[ProcessBioMetricSwipesForFlatTable](https://github.com/zinghrcore/z2-tna-db/blob/master/%5BTNA%5D.%5BProcessBioMetricSwipesForFlatTable%5D)

**⏲️ Scheduled Jobs**
- Step1:Daily Flat Table Creation Job which is Scheduled Once daily. (e.g., at 11:30 PM) using step5.
- Step2:Biometric Punch Processing Job which is Scheduled Every 10 minutes (24x7) using step 10.

# 🕒 BackDated Attendance Process - TNA System

This guide outlines the sequential process for executing **BackDated Attendance updation** in the **Time and Attendance (TNA)** system.

## 📌 Job Reference  
**Job Name:** `Attendance_job_for_cowayqa5`  
**Objective:** To process and finalize backdated attendance records, ensuring rule compliance and data readiness for reporting.
### ✅ Step 1:[Generate the FlatTable required for backdated data processing.](https://github.com/zinghrcore/z2-tna-db/blob/master/1.setempshiftjsondata.sql)
### ✅ Step 2:[Create FILO (First In Last Out) attendance records from the JSON shift data](https://github.com/zinghrcore/z2-tna-db/blob/master/2.FiloCreationWithJSON_Z2.sql)
### ✅ Step 3:[Apply Working Hours Rule by creating sp [TNA].[AttendanceRule_WorkingHrs]](https://github.com/zinghrcore/z2-tna-db/blob/master/3.AttendanceRule_WorkingHrs.sql)
### ✅ Step 4:[Apply Late Coming Rule using sp ATTENDANCERULE_LATECOMING](https://github.com/zinghrcore/z2-tna-db/blob/master/4.ATTENDANCERULE_LATECOMING.sql)
### ✅ Step 5:[Apply Late Coming Rule using sp ATTENDANCERULE_EARLYGOING](https://github.com/zinghrcore/z2-tna-db/blob/master/5.ATTENDANCERULE_EARLYGOING.sql)
### ✅ Step 6:[Apply Flexitime Rule using sp ATTENDANCERULE_FLEXITIME](https://github.com/zinghrcore/z2-tna-db/blob/master/6.ATTENDANCERULE_FLEXITIME.sql)
### ✅ Step 7:[Process BackDated Extra Time using sp GenExtraTimeZ2_AttProcess](https://github.com/zinghrcore/z2-tna-db/blob/master/7.GenExtraTimeZ2_AttProcess.sql)
### ✅ Step 8:[Apply Compensatory Off Rule usinf sp ATTENDANCERULE_COMPOFF](https://github.com/zinghrcore/z2-tna-db/blob/master/8.ATTENDANCERULE_COMPOFF.sql)
### ✅ Step 9:[Handle Attendance Exceptions using sp ATTENDANCERULE_EXCEPTIONS](https://github.com/zinghrcore/z2-tna-db/blob/master/9.ATTENDANCERULE_EXCEPTIONS.sql)
### ✅ Step 10:[ Display Attendance Results using sp AttendanceRule_DisplayResult](https://github.com/zinghrcore/z2-tna-db/blob/master/10.AttendanceRule_DisplayResult.sql)
### ✅ Step 11:[Transfer Data for Reports using sp DataTransferForReports](https://github.com/zinghrcore/z2-tna-db/blob/master/11.DataTransferForReports.sql)

## 🌟 Contributors

<table>
  <tr>
    <td align="center">
      <a href="https://www.linkedin.com/in/prasad-rajappan-a002a73/" target="_blank">
        <img src="https://media.licdn.com/dms/image/v2/C4E03AQEQl64iTddLkw/profile-displayphoto-shrink_400_400/profile-displayphoto-shrink_400_400/0/1516298618284?e=1751500800&v=beta&t=QZ-WYMxK5vPV-_iFCikorpW6VSIWnhWAz7LlXiX5LXE" width="80px" style="border-radius:50%;" alt="Contributor 1"/>
        <br/>
        <sub><b>Prasad Rajappan</b></sub>
      </a>
    </td>
    <td align="center">
      <a href="https://www.linkedin.com/in/nikhil004/" target="_blank">
        <img src="https://media.licdn.com/dms/image/v2/D4D03AQGys4LpBZOvng/profile-displayphoto-shrink_200_200/profile-displayphoto-shrink_200_200/0/1726168691780?e=2147483647&v=beta&t=7_LfxXThuPlIpSHmiPCQe1bwPCkJW52oAVhJOn5FL0E" width="80px" style="border-radius:50%;" alt="Contributor 2"/>
        <br/>
        <sub><b>Nikhil Mishra</b></sub>
      </a>
    </td>
        <td align="center">
      <a href="https://www.linkedin.com/in/hemant-meena-208b2556/" target="_blank">
        <img src="https://media.licdn.com/dms/image/v2/D4D03AQE7YLjE5a77dg/profile-displayphoto-shrink_400_400/B4DZXlkzApHsAg-/0/1743313384104?e=1751500800&v=beta&t=fMEISdFGYxEw5J4Wnki5WaBSIIsz9yD4aHsmx0F3Bq8" width="80px" style="border-radius:50%;" alt="Contributor 4"/>
        <br/>
        <sub><b>Hemant Meena</b></sub>
      </a>
    </td>
    <td align="center">
      <a href="https://www.linkedin.com/in/mihir-mistry-93068b223/" target="_blank">
        <img src="https://media.licdn.com/dms/image/v2/D4D03AQEpLW8pg6DVgw/profile-displayphoto-shrink_200_200/B4DZRrVZvRHcAg-/0/1736967562482?e=2147483647&v=beta&t=HrPYZoofkqgqDIfavB3QjqSbbWQPP4aza3LomSTXoGk" width="80px" style="border-radius:50%;" alt="Contributor 3"/>
        <br/>
        <sub><b>Mihir Mistry</b></sub>
      </a>
    </td>
  </tr>
  <tr>
    <td align="center">
      <a href="https://www.linkedin.com/in/amol-dingankar-315459121/" target="_blank">
        <img src="https://media.licdn.com/dms/image/v2/D4D03AQHvWF1_C18zxw/profile-displayphoto-shrink_400_400/profile-displayphoto-shrink_400_400/0/1670330113667?e=1751500800&v=beta&t=5OHQubiCZG5QdtQSh6AiQinKHsmllf0XGrw2baotTsk" width="80px" style="border-radius:50%;" alt="Contributor 5"/>
        <br/>
        <sub><b>Amol Dingankar</b></sub>
      </a>
    </td>
        <td align="center">
      <a href="https://www.linkedin.com/in/omveer-singh-82102a29/" target="_blank">
        <img src="https://media.licdn.com/dms/image/v2/D5603AQEAaLfXazpQeA/profile-displayphoto-shrink_400_400/profile-displayphoto-shrink_400_400/0/1714208348267?e=1751500800&v=beta&t=l5QotBO0eGPa5Nman6hlqQu6A5xPoVjMbPn8Ua6Ac84" width="80px" style="border-radius:50%;" alt="Contributor 7"/>
        <br/>
        <sub><b>Omveer Singh</b></sub>
      </a>
    </td>
    <td align="center">
      <a href="https://www.linkedin.com/in/vijay-boura-b1197517a/" target="_blank">
        <img src="https://media.licdn.com/dms/image/v2/D5603AQFFl0UQvFIiuw/profile-displayphoto-shrink_400_400/B56ZaQunylHsAg-/0/1746184866189?e=1751500800&v=beta&t=-oPo7evoLYQwDDSuPYLHlfmqTAnQVLJu5R1B8nRLgJo" width="80px" style="border-radius:50%;" alt="Contributor 6"/>
        <br/>
        <sub><b>Vijay Boura</b></sub>
      </a>
    </td>
    <td align="center">
      <a href="https://www.linkedin.com/in/imrohi8/" target="_blank">
        <img src="https://media.licdn.com/dms/image/v2/D5603AQG5hOhdd_j3xg/profile-displayphoto-shrink_800_800/B56ZUkf5m7HQAc-/0/1740074097527?e=1756339200&v=beta&t=FcnQHqj_0DvfeI9F7P_b8lex4IyeztmFtFqtUusi6Uo" width="80px" style="border-radius:50%;" alt="Contributor 8"/>
        <br/>
        <sub><b>Rohit Raj</b></sub>
      </a>
    </td>
  </tr>
</table>






## 📁 Resources

- 📄 [Detailed Report](https://zinghr365-my.sharepoint.com/:w:/g/personal/nikhil_mishra_zinghr_com/EQtXlIN-tVFKks0tMhePZmEBjwJWUVPBQbnlyqqT4rnOJQ?wdOrigin=TEAMS-MAGLEV.undefined_ns.rwc&wdExp=TEAMS-TREATMENT&wdhostclicktime=1746181572438&web=1)
- 📄 [Steps Report](https://drive.google.com/file/d/1VrXBYQQknR33bqLG-Che7I3bnBFKTl6T/view?usp=drive_link)
- 📄 [All Tables and Stored Procedures](https://github.com/zinghrcore/z2-tna-db/blob/master/Final%20SP%20and%20tables.rar)
- 📄 [BackDated Attendance Process](https://github.com/zinghrcore/z2-tna-db/blob/master/BackDated%20Attendance%20Process.docx)
  
[🔝 Back to Top](#readme-top)

Copyright © 2025[ZingHR](https://www.zinghr.com/)  <br />










