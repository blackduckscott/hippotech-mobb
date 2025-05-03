# Command Injection fix breaks Demo App

The generated fix can be seen in this commit: [f342904](https://github.com/blackduckscott/hippotech-mobb/commit/f3429047db4fba8ed1f3a4aa654d1bf1699ea9bd)

To run the app:

```bash
mvn clean package
java -jar target/api.jar
```

To run the tests:

```bash
./run-tests.sh
```

Normally these tests all pass but with the fix one will fail:

```
(base) ➜  hippotech-mobb git:(main) ✗ ./run-tests.sh
 ...
 PASS  ./blog.spec.js
 PASS  ./myMortgages.test.js
 PASS  ./login.test.js
 FAIL  ./mortgageApproval.test.js            <---------------
  ● Mortgage Approval › can be requested
```

There is a runtime exception that I *think* occurs because the newly added dependency has a version that does not line up with other Apache commons libraries. 

```
; nested exception is java.lang.NoSuchMethodError: 'org.apache.commons.lang3.Range org.apache.commons.lang3.Range.of(java.lang.Comparable, java.lang.Comparable)'] with root cause

java.lang.NoSuchMethodError: 'org.apache.commons.lang3.Range org.apache.commons.lang3.Range.of(java.lang.Comparable, java.lang.Comparable)'
	at org.apache.commons.text.translate.NumericEntityEscaper.<init>(NumericEntityEscaper.java:97) ~[commons-text-1.13.1.jar!/:1.13.1]
	at org.apache.commons.text.translate.NumericEntityEscaper.between(NumericEntityEscaper.java:59) ~[commons-text-1.13.1.jar!/:1.13.1]
	at org.apache.commons.text.StringEscapeUtils.<clinit>(StringEscapeUtils.java:270) ~[commons-text-1.13.1.jar!/:1.13.1]
	at com.hippotech.api.controllers.MortgageApprovalController.postApprovalRequest(MortgageApprovalController.java:130) ~[classes!/:0.0.1-SNAPSHOT]
	at jdk.internal.reflect.DirectMethodHandleAccessor.invoke(DirectMethodHandleAccessor.java:104) ~[?:?]
	at java.lang.reflect.Method.invoke(Method.java:578) ~[?:?]
```

