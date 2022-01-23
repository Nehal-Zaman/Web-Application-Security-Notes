# Lab: Information disclosure in error messages

```bash
┌──(kali㉿kali)-[~]
└─$ curl https://ac6c1f581e2a3a09c0dd11f4005600f5.web-security-academy.net/product?productId=test
Internal Server Error: java.lang.NumberFormatException: For input string: "test"
        at java.base/java.lang.NumberFormatException.forInputString(NumberFormatException.java:67)
        at java.base/java.lang.Integer.parseInt(Integer.java:668)
        at java.base/java.lang.Integer.parseInt(Integer.java:786)
        at lab.i.b.s.u.S(Unknown Source)
        at lab.display.i.r.o.x(Unknown Source)
        at lab.display.i.e.a.w.h(Unknown Source)
        at lab.display.i.e.e.lambda$handleSubRequest$0(Unknown Source)
        at l.a.a.x.lambda$null$3(Unknown Source)
        at l.a.a.x.t(Unknown Source)
        at l.a.a.x.lambda$uncheckedFunction$4(Unknown Source)
        at java.base/java.util.Optional.map(Optional.java:260)
        at lab.display.i.e.e.j(Unknown Source)
        at lab.y.j.y.o.h(Unknown Source)
        at lab.display.i.p.h(Unknown Source)
        at lab.y.j.y.p.D(Unknown Source)
        at lab.y.j.y.p.t(Unknown Source)
        at l.a.a.x.lambda$null$3(Unknown Source)
        at l.a.a.x.t(Unknown Source)
        at l.a.a.x.lambda$uncheckedFunction$4(Unknown Source)
        at lab.y.i5.U(Unknown Source)
        at lab.y.j.y.p.y(Unknown Source)
        at lab.y.j.w.b.E(Unknown Source)
        at lab.y.j.h.b(Unknown Source)
        at lab.y.d.W(Unknown Source)
        at lab.y.d.N(Unknown Source)
        at lab.y.d.i(Unknown Source)
        at l.a.c.t.hs.P(Unknown Source)
        at l.a.c.t.hs.B(Unknown Source)
        at l.a.c.t.hs.run(Unknown Source)
        at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1136)
        at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:635)
        at java.base/java.lang.Thread.run(Thread.java:833)

Apache Struts 2 2.3.31
```

