---
date:
  created: 2025-01-07
categories:
  - Car
tags:
  - Technology
  - Life updates 
authors:
  - why
---



# 纪念日

自2025年8月2日以来，已经过去了：<span id="anniversary-timer"></span>

<script>
  function updateAnniversaryTimer() {
    const anniversaryDate = new Date("2025-08-02T00:00:00Z");
    const now = new Date();
    const diffMs = now.getTime() - anniversaryDate.getTime();

    const seconds = Math.floor(diffMs / 1000);
    const minutes = Math.floor(seconds / 60);
    const hours = Math.floor(minutes / 60);
    const days = Math.floor(hours / 24);
    const years = Math.floor(days / 365.25); // Account for leap years

    const remainingDays = days - Math.floor(years * 365.25);
    const remainingHours = hours % 24;
    const remainingMinutes = minutes % 60;
    const remainingSeconds = seconds % 60;

    document.getElementById("anniversary-timer").innerText = 
      `${years} 年 ${remainingDays} 天 ${remainingHours} 小时 ${remainingMinutes} 分钟 ${remainingSeconds} 秒`;
  }

  setInterval(updateAnniversaryTimer, 1000);
  updateAnniversaryTimer(); // Initial call to display immediately
</script>
