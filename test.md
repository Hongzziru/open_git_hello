#Python(csv module) 과 Java(Jackson lib) CSV Read/Write Performance Test

###Test Environment
 CPU : Intel(R) Core(TM) i5-6200U CPY @ 2.30GHz 2.40 GHz
 RAM : 8.00 GB
 OS : Windows 10 64bit
 Storage : 256GB SSD
 JDK : 1.8.0_25 64-bit
 Pyhon : 2.7.11
 
###Test Case
Col : 50
Row : 100,000 / 400,000 / 700,000 / 1,000,000
Comma : Col - 0% / 20% / 50%
ex) a,a,a,a,'a,',a,a,a,a,'a,'...

>Test는 I/O로 인한 측정 시간 증가로 메모리 내에서 진행

###Python code [PyCharm Community Edition 2016.1.2]
```python
import cStringIO as StringIO
import csv
import time

column = []
record = []

col = 0
while col / 50 == 0:
    # column.append('a')
    col += 1

#-------record case----
#   0%
#     column.append('a')

  # 20%
  #   if col%5 == 0:
  #       column.append('c,')
  #   else:
  #       column.append('a')

#   50%
    if col%2 == 0:
        column.append('c,')
    else:
        column.append('a')

#--------size case----
#   05MB : 5242880
#   10MB : 10485760
#   20MB : 20971520
#   50MB : 52428800
#   100MB : 104857600
#   200MB : 209715200

readTime = []
writeTime = []

# while record.__sizeof__() < 5242880:
while record.__len__() < 150000:
    record.append(column)

print 'record size : ', record.__sizeof__() / 1024 / 1024

i = 0
while True:
    if i == 10:
        break
    f = StringIO.StringIO()
    cw = csv.writer(f)

    # Write
    st = time.time()
    cw.writerows(record)
    ed = time.time()
    print 'Write time : ', ed - st
    writeTime.append(ed-st)

    get_str = f.getvalue()

    # Read
    st = time.time()
    a = ",".join(get_str)
    ed = time.time()
    print 'Read time : ', ed-st
    readTime.append(ed-st)
    f.close()
    get_str = 0
    a = 0
    i += 1
```
###Java code [IntelliJ IDEA 2016.1.1]
```java
import com.fasterxml.jackson.databind.MappingIterator;
import com.fasterxml.jackson.databind.ObjectWriter;
import com.fasterxml.jackson.dataformat.csv.CsvMapper;
import com.fasterxml.jackson.dataformat.csv.CsvParser;
import com.fasterxml.jackson.dataformat.csv.CsvSchema;
import com.sun.xml.internal.messaging.saaj.util.ByteInputStream;
import com.sun.xml.internal.messaging.saaj.util.ByteOutputStream;

import java.io.IOException;
import java.util.ArrayList;
import java.util.LinkedList;
import java.util.List;

/**
 * Created by User on 2016-04-19.
 */
public class jacksonTest {
    public static void main(String args[]) throws IOException {
        List datas = new ArrayList();
        while(true){
////            0%
//            Data d = new Data("a","a","a","a","a","a","a","a","a","a","a","a",
//                              "a","a","a","a","a","a","a","a","a","a","a","a",
//                              "a","a","a","a","a","a","a","a","a","a","a","a",
//                              "a","a","a","a","a","a","a","a","a","a","a","a","a","a");

////            20%
//            Data d = new Data("a,","a","a","a","a","a,","a","a","a","a","a,",
//                              "a","a","a","a","a,","a","a","a","a","a,","a","a",
//                              "a","a","a,","a","a","a","a","a,","a","a","a","a",
//                              "a,","a","a","a","a","a,","a","a","a","a","a,","a","a","a","a");

////            50%
            Data d = new Data("a,","a,","a,","a,","a,","a","a","a","a","a","a,",
                    "a,","a,","a,","a,","a","a","a","a","a","a,","a,","a,","a,","a,",
                    "a","a","a","a","a","a,","a,","a,","a,","a,","a","a","a","a","a",
                    "a,","a,","a,","a,","a,","a","a","a","a","a");

            datas.add(d);
            if(datas.size()>150000) break;
        }

        System.out.println((Runtime.getRuntime().totalMemory()-Runtime.getRuntime().freeMemory())/1024/1024);
        System.out.println("make Data End    Data size : " + datas.size());

    int i=0;
    while(true) {
        if(i==10) break;
        i++;
        ByteInputStream bi = new ByteInputStream();
        ByteOutputStream bo = new ByteOutputStream();

        CsvMapper mapper = new CsvMapper();
        CsvSchema schema = mapper.schemaFor(Data.class);
        schema = schema.withColumnSeparator(',').withLineSeparator("\n");

        ObjectWriter writer = mapper.writer(schema);


        long start = System.currentTimeMillis();
        writer.writeValue(bo, datas);
        long end = System.currentTimeMillis();
        System.out.println("Write Time : " + (end - start) / 1000.0);
//        System.out.println("bo size : "+ bo.size());
//        System.out.println("Write end");

        CsvMapper mapper2 = new CsvMapper();
        mapper2.enable(CsvParser.Feature.WRAP_AS_ARRAY);

        bi = bo.newInputStream();

        long start2 = System.currentTimeMillis();
        MappingIterator it = mapper2.readerFor(String[].class).readValues(bi);
        while (it.hasNext()) {
            String[] row = it.next();
        }
        long end2 = System.currentTimeMillis();
        System.out.println("Read Time : " + (end2 - start2) / 1000.0);

        bi.close();
        bi = null;

        bo.close();
        bo = null;
        it = null;
        writer = null;

    }


    }
}
```

###Python vs Java  CSV read Time

###Python vs Java  CSV write Time

###Result

read : java > python 
write : python > java
