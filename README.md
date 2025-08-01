<a name="readme-top"></a>

📘 Project Overview: <br>
The Real-Time Attendance (TNA) system is designed to automate and streamline employee attendance processing using biometric swipes data pr Mobile Punches. It integrates with our RTA Engine to ensure accurate, rule-based attendance status updates and compliance across various scenarios, such as shifts, late/early punches, OT and backdated corrections(if applicable).

🔧 Core Functionalities: <br>
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

🆕 Automated Attendance Processing
Real-time tracking and processing eliminate manual intervention, reducing human errors.

🆕 Overtime Management
Calculates PreOT, PostOT, RDOT(RestDay OT), SHOT(Special Holiday OT), LHOT(LegalHoliday OT), and NDOT(Normal Day OT) based on shift definitions and daytype(holiday/rest/working).

🆕 Grace Period Validation
Dynamically tracks and restricts late-in/early-out entries based on allowed counts and duration, triggering Half Day or Absent statuses on violations.

🆕 Single Swipe Handling
Single Swipe: Marked Present if employee is mapped for single swipe attendance; otherwise Absent unless regularization/leave/OD exists.

🆕 No Swipe Handling: Marked Present for high-level officers if mapped; otherwise Absent unless on leave/holiday/off.

🆕 Flexi Shift Support
Handles cross-day punch conditions intelligently, ensuring correct attendance for employees with flexible working patterns.

🆕 Shift Capping
Adjusts punch times to align with capped shift windows (e.g., early punches capped to 10:00 AM, late punches to 6:30 PM) for consistency.

🆕 Night Differential Calculation
Calculates hours worked between 10:00 PM and 6:00 AM, especially for Southeast Asia-based operations. 

🆕 Productive Hours (CKC)
Only valid IN-OUT swipe pairs in correct sequence (no interruptions) are counted. Productive hours exclude tea breaks, outings, and other breaks. Unmatched swipes trigger regularization alerts.

🆕 Weekly Attendance Shortfall Detection
Calculates shortfall in total weekly working hours for compliance, particularly for Jio employees.

🆕 L1 Payroll Integration
Provides pre-processed OT and ND metrics as input to payroll systems  with specific parameters like RDOT, NDHours, SHOTExcess, etc.

🆕 Leave Sandwiching
Implements sandwich rules that apply Rest Days or Off Days within a leave period as paid leave, based on configurations.


🛠️ Real-Time Attendance (TNA) Setup Steps for New Client/Database: <br>
Step 1: Duplicate the live database with the latest group,shift mappings and latest configured Rules.

Step 2: Deploy patches for:New Rostering Screen,New Setup Screen and All required Microservices.

Step 3: Synchronize live biometric swipes into the new database.

Step 4: Execute all latest scripts (to be shared via Zira ID).

Step 5: Configure OT slabs using the new Setup Screen.

Step 6: Setup all required jobs, including:Flat table creation,Extra time JSON creation,No swipes processing and Biometric swipe processing.

Step 7: Update ProcessBiometricSwipesForFlatTable with the last processed swipe(Attendance jobs will process from this point onward).

**⏲️ Scheduled Jobs**
- Step1:Daily Flat Table Creation Job which is Scheduled Once daily. (e.g., at 11:30 PM) using step5.
- Step2:Biometric Punch Processing Job which is Scheduled Every 10 minutes (24x7) using step 10.

# 🕒 BackDated Attendance Process - TNA System

This guide outlines the sequential process for executing **BackDated Attendance updation** in the **Time and Attendance (TNA)** system.

## 📌 Job Reference  
**Job Name:** `Backdated_Attendance_job_for`  
**Objective:** To process and finalize backdated attendance records, ensuring rule compliance and data readiness for reporting.
✅ Step 1: Generate the FlatTable required for backdated data processing
EXEC [TNA].[SetEmpShiftJSONData]

✅ Step 2: Create FILO (First In Last Out) attendance records from the JSON shift data
EXEC [TNA].[FiloCreationWithJSON_Z2]

✅ Step 3: Apply Working Hours Rule by creating sp [TNA].[AttendanceRule_WorkingHrs]
EXEC [TNA].[AttendanceRule_WorkingHrs]

✅ Step 4: Apply Late Coming Rule using sp ATTENDANCERULE_LATECOMING
EXEC [TNA].[ATTENDANCERULE_LATECOMING]

✅ Step 5: Apply Early Going Rule using sp ATTENDANCERULE_EARLYGOING
EXEC [TNA].[ATTENDANCERULE_EARLYGOING]

✅ Step 6: Apply Flexitime Rule using sp ATTENDANCERULE_FLEXITIME
EXEC [TNA].[ATTENDANCERULE_FLEXITIME]

✅ Step 7: Process BackDated Extra Time using sp GenExtraTimeZ2_AttProcess
EXEC [TNA].[GenExtraTimeZ2_AttProcess]

✅ Step 8: Apply Compensatory Off Rule using sp ATTENDANCERULE_COMPOFF
EXEC [TNA].[ATTENDANCERULE_COMPOFF]

✅ Step 9: Handle Attendance Exceptions using sp ATTENDANCERULE_EXCEPTIONS
EXEC [TNA].[ATTENDANCERULE_EXCEPTIONS]

✅ Step 10: Display Attendance Results using sp AttendanceRule_DisplayResult
EXEC [TNA].[AttendanceRule_DisplayResult]

✅ Step 11: Transfer Data for Reports using sp DataTransferForReports
EXEC [TNA].[DataTransferForReports]
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










