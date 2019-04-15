---
title: "Parsing dynamic json values using iterator"
author: "Tobibur"
---

---

### The problem:
We have a json value with key-value pairs as shown below:
```json
{
	"status": "success",
	"response": {
		"data": {
			"currentScore": {
				"1": {
					"score": 27
				},
				"2": {
					"score": 38
				}
			},
			"totalScore": {
				"1": 20258,
				"2": 20380
			}
		}
	}
}
```

As we can see that the currentScore and the totalScore JSONObjects has dynamic key values i.e, 1,2..etc. Now what the problem here is that we can't parse the values from the keys directly like ```currentScore.getJSONObject("1")``` as the key value may change in case of addition or deletion of json data.

---

### The Solution:
So, The Iterator comes to the rescue. We use Iterator to solve this as follows:

```Java
import org.json.JSONException;
import org.json.JSONObject;

import java.util.Iterator;

public class SampleJson {

    public static void main(String[] args) {

        String jsonValue = "{\"status\":\"success\",\"response\":{\"data\":" +
                "{\"currentScore\":{\"1\":{\"score\":27},\"2\":{\"score\":38}}," +
                "\"totalScore\":{\"1\":20258,\"2\":20380}}}}\n";

        try {
            JSONObject jsonObject = new JSONObject(jsonValue);
            JSONObject currentScore = jsonObject.getJSONObject("response").getJSONObject("data")
                    .getJSONObject("currentScore");
            JSONObject totalScore = jsonObject.getJSONObject("response").getJSONObject("data")
                    .getJSONObject("totalScore");

            Iterator<String> iterator = currentScore.keys();
            while (iterator.hasNext()) {
                String user_key = iterator.next();
                System.out.println("key = " + user_key);
                JSONObject score = currentScore.getJSONObject(user_key);
                System.out.println("score ->" + score.getInt("score"));
                System.out.println("total score ->" + totalScore.getInt(user_key) + "\n");
            }
        } catch (JSONException e) {
            e.printStackTrace();
        }
    }
}
```

---

### The Explaination
In the above java code, we defined a String variable to store the json value for this example. Then inside the try catch we initialized the JSONObject with the jsonValue and retrieved the currentScore and totalScore JSONObjects. Now, we initialized the Iterator object with the keys of currentScore.
```Java
 Iterator<String> iterator = currentScore.keys();
 ```
 Then we added a while loop looping through the iterator key values with the method ```iterator.hasNext()```. And retrieved the dynamic key value with ```iterator.next()``` method. And finally we can successfully retrieve the value from the dynamic key using ```JSONObject score = currentScore.getJSONObject(user_key);``` method.
```Java
Iterator<String> iterator = currentScore.keys();
while (iterator.hasNext()) {
    String user_key = iterator.next();
    System.out.println("key = " + user_key);
    JSONObject score = currentScore.getJSONObject(user_key);
    System.out.println("score ->" + score.getInt("score"));
    System.out.println("total score ->" + totalScore.getInt(user_key) + "\n");
}
```

---

### The output:
Therefore, the output of the above program will be as shown below:

>Output

*key = 1  
score ->27  
total score ->20258*

*key = 2  
score ->38  
total score ->20380*

---

### The End
Thank you :smiley:


