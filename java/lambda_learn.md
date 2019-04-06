java 8 lambda表达式使用小记
===========================
业务需求：从数据库查询到结果是由';'分割的JSONArray字符串，形式上为
[{"start_time": "08:01:10", "end_time": "18:11:20", "service_name": "mickey_detect"},{"start_time": "10:01:02", "end_time": "20:20:30", "service_name": "chef_detect", "duration": 10}];[{"start_time": "00:01:10", "end_time": "06:11:20", "service_name": "mickey_detect"},{"start_time": "08:11:00", "end_time": "20:20:30", "service_name": "chef_detect", "duration": 20}]

```javas
import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONArray;
import com.alibaba.fastjson.JSONObject;
import com.alibaba.fastjson.annotation.JSONField;
import org.apache.commons.lang3.StringUtils;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.regex.Pattern;
import java.util.stream.Collectors;

public class Config {
  @JSONField(name = "start_time")
  private int startTime;
  
  @JSONField(name = "end_time")
  private int endTime;
    
  @JSONField(name = "service_name")
  private String serviceName;
  
  private int time2Second(String timeStr) {
    return Arrays.stream(timeStr.split(Pattern.quote(":")))
    .mapToInt(Integer::valueOf)
    .reduce((sum, item) -> sum * 60 + item)
    .getAsInt();;
  }
    
  public void setStartTime(String startTime) {
    this.startTime = time2Second(startTime);
  }
    
  public void setEndTime(String endTime) {
    this.endTime = time2Second(endTime);
  }
    
  public void setServiceName(String serviceName) {
    this.serviceName = serviceName;
  }
    
  public int getStartTime() {
    return startTime;
  }
    
  public int getEndTime() {
    return endTime;
  }
    
  public String getServiceName() {
    return serviceName;
  }
}
```
