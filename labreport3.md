# Part 1 - Bugs
## Failure inducing input  
 `@Test
  public void testReversed2() {
    int[] input1 = {1, 2, 3};
    assertArrayEquals(new int[]{3, 2, 1}, ArrayExamples.reversed(input1));
  }`  
## Non-failure inducing input  
`@Test
  public void testReversed3() {
    int[] input1 = {0, 0, 0};
    assertArrayEquals(new int[]{0, 0, 0}, ArrayExamples.reversed(input1));
  }`  
## Symptom  
`1) testReversed2(ArrayTests)
arrays first differed at element [0]; expected:<3> but was:<0>
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:78)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:28)
        at org.junit.Assert.internalArrayEquals(Assert.java:534)
        at org.junit.Assert.assertArrayEquals(Assert.java:418)
        at org.junit.Assert.assertArrayEquals(Assert.java:429)
        at ArrayTests.testReversed2(ArrayTests.java:21)
        ... 32 trimmed
Caused by: java.lang.AssertionError: expected:<3> but was:<0>
        at org.junit.Assert.fail(Assert.java:89)
        at org.junit.Assert.failNotEquals(Assert.java:835)
        at org.junit.Assert.assertEquals(Assert.java:120)
        at org.junit.Assert.assertEquals(Assert.java:146)
        at org.junit.internal.ExactComparisonCriteria.assertElementsEqual(ExactComparisonCriteria.java:8)
        at org.junit.internal.ComparisonCriteria.arrayEquals(ComparisonCriteria.java:76)
        ... 38 more`  
## Bug  
Before:  
`static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = newArray[arr.length - i - 1];
    }
    return arr;
  }`  
  After:  
  `static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      newArray[i] = arr[arr.length - i - 1];
    }
    return newArray;
  }`  
    
 This fix adresses the issue because instead of changing the old array to all zeros, it changes the new array to the reversed values of the old array.  
    
# Part 2 - Researching commands - find  
where I found the commands : https://tecadmin.net/linux-find-command-with-examples/

## find -type
This command finds only a certain type of path, such as files or directories, that you specify. This is useful if you are only lokking for a certain type of path within a directory.    
`sophiasanat@MacBook-Pro-83 docsearch-1 % find technical/911report -type d`  
`technical/911report` 
  
`sophiasanat@MacBook-Pro-83 docsearch-1 % find technical/911report -type f`  
`technical/911report/chapter-13.4.txt
technical/911report/chapter-13.5.txt
technical/911report/chapter-13.1.txt
technical/911report/chapter-13.2.txt
technical/911report/chapter-13.3.txt
technical/911report/chapter-3.txt
technical/911report/chapter-2.txt
technical/911report/chapter-1.txt
technical/911report/chapter-5.txt
technical/911report/chapter-6.txt
technical/911report/chapter-7.txt
technical/911report/chapter-9.txt
technical/911report/chapter-8.txt
technical/911report/preface.txt
technical/911report/chapter-12.txt
technical/911report/chapter-10.txt
technical/911report/chapter-11.txt`  

## find -size
This command finds only files under, over or exactly the size you specify. This is useful because you could be looking for only smaller or bigger files.  

`sophiasanat@MacBook-Pro-83 docsearch-1 % find technical -size +200k`  
`technical/government/About_LSC/commission_report.txt
technical/government/Env_Prot_Agen/bill.txt
technical/government/Gen_Account_Office/GovernmentAuditingStandards_yb2002ed.txt
technical/government/Gen_Account_Office/Statements_Feb28-1997_volume.txt
technical/government/Gen_Account_Office/d01591sp.txt
technical/911report/chapter-13.4.txt
technical/911report/chapter-13.5.txt
technical/911report/chapter-3.txt`  

`sophiasanat@MacBook-Pro-83 docsearch-1 % find technical -size -1k `  
`technical
technical/government
technical/government/About_LSC
technical/government/Env_Prot_Agen
technical/government/Alcohol_Problems
technical/government/Post_Rate_Comm
technical/plos/pmed.0020191.txt
technical/plos/pmed.0020226.txt
technical/911report`  

## find -mtime
This command finds only files modified or not modified in the specified amount of time. This useful if you want to find files that you know when they were modified.  

`sophiasanat@MacBook-Pro-83 docsearch-1 % find technical -mtime +1`  
`sophiasanat@MacBook-Pro-83 docsearch-1 %`    

`sophiasanat@MacBook-Pro-83 docsearch-1 % find technical/government/About_LSC -mtime -1`  
`technical/government/About_LSC
technical/government/About_LSC/LegalServCorp_v_VelazquezSyllabus.txt
technical/government/About_LSC/Progress_report.txt
technical/government/About_LSC/Strategic_report.txt
technical/government/About_LSC/Comments_on_semiannual.txt
technical/government/About_LSC/Special_report_to_congress.txt
technical/government/About_LSC/CONFIG_STANDARDS.txt
technical/government/About_LSC/commission_report.txt
technical/government/About_LSC/LegalServCorp_v_VelazquezDissent.txt
technical/government/About_LSC/ONTARIO_LEGAL_AID_SERIES.txt
technical/government/About_LSC/LegalServCorp_v_VelazquezOpinion.txt
technical/government/About_LSC/diversity_priorities.txt
technical/government/About_LSC/reporting_system.txt
technical/government/About_LSC/State_Planning_Report.txt
technical/government/About_LSC/Protocol_Regarding_Access.txt
technical/government/About_LSC/ODonnell_et_al_v_LSCdecision.txt
technical/government/About_LSC/conference_highlights.txt
technical/government/About_LSC/State_Planning_Special_Report.txt`  

## find -user
This command finds files based on the owner. This is useful if you are only looking for files or looking for all the files with a certain owner.

`sophiasanat@MacBook-Pro-83 docsearch-1 % find technical/government/About_LSC -user sophia`           
`find: -user: sophia: no such user`   

`sophiasanat@MacBook-Pro-83 docsearch-1 % find technical/government/About_LSC -user SophiaSanat`  
`technical/government/About_LSC
technical/government/About_LSC/LegalServCorp_v_VelazquezSyllabus.txt
technical/government/About_LSC/Progress_report.txt
technical/government/About_LSC/Strategic_report.txt
technical/government/About_LSC/Comments_on_semiannual.txt
technical/government/About_LSC/Special_report_to_congress.txt
technical/government/About_LSC/CONFIG_STANDARDS.txt
technical/government/About_LSC/commission_report.txt
technical/government/About_LSC/LegalServCorp_v_VelazquezDissent.txt
technical/government/About_LSC/ONTARIO_LEGAL_AID_SERIES.txt
technical/government/About_LSC/LegalServCorp_v_VelazquezOpinion.txt
technical/government/About_LSC/diversity_priorities.txt
technical/government/About_LSC/reporting_system.txt
technical/government/About_LSC/State_Planning_Report.txt
technical/government/About_LSC/Protocol_Regarding_Access.txt
technical/government/About_LSC/ODonnell_et_al_v_LSCdecision.txt
technical/government/About_LSC/conference_highlights.txt`
