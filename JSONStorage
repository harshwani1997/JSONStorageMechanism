import java.io.*;
import java.util.*;
import java.text.*;
import java.math.*;
import java.util.regex.*;
import org.json.simple.JSONArray;
import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;
import org.json.simple.parser.ParseException;
import org.json.*;

public class storage {

    static int docIndex=0;
    public static void main(String args[] ) {
        /* Enter your code here. Read input from STDIN. Print output to STDOUT */
        HashMap<Integer, String> documents;
        HashMap<String, HashMap<String, List<Integer>>> store;
        try {
            BufferedReader in = new BufferedReader(new InputStreamReader(System.in));

            store = new HashMap<>();
            documents = new HashMap<>();

            String s;
            while((s=in.readLine()) !=null){
                String[] arr = s.split(" ", 2);
                if("add".equals(arr[0])) {
                    addJSON(arr[1], documents, store);
                } else if("get".equals(arr[0])) {
                    JSONParser parser = new JSONParser();
                    JSONObject target = (JSONObject) parser.parse(arr[1]);

                    List<List<Integer>> lists = new ArrayList<>();
                    findJSON(target, lists, store);

                    if(lists.isEmpty()){
                        for(int i:documents.keySet()){
                            System.out.println(documents.get(i));
                        }
                        continue;
                    }

                    //get intersection
                    List<Integer> intersection = lists.get(0);
                    for(int i=1;i<lists.size();i++){
                        intersection = getIntersection(intersection, lists.get(i));
                    }

                    for(int i:intersection){
                        if(documents.containsKey(i))
                        {
                            System.out.println(documents.get(i));
                        }
                    }
                }
                else if("delete".equals(arr[0])) {
                    JSONParser parser = new JSONParser();
                    JSONObject target = (JSONObject) parser.parse(arr[1]);

                    List<List<Integer>> lists = new ArrayList<>();
                    findJSON(target, lists, store);

                    //get intersection
                    List<Integer> intersection = lists.get(0);
                    for(int i=1;i<lists.size();i++){
                        intersection = getIntersection(intersection, lists.get(i));
                    }

                    for(int i:intersection){
                        documents.remove(i);
                    }
                }
            }
        } catch(Exception e){
            e.printStackTrace();
        }

    }

    public static void findJSON(JSONObject json, List<List<Integer>> lists, HashMap<String, HashMap<String, List<Integer>>> store)
    {
        Set<String> querySet = json.keySet();
        for(String query:querySet){
            if(json.get(query) instanceof JSONObject)
            {
                findJSON((JSONObject)json.get(query), lists, store);
            }
            else if(json.get(query) instanceof JSONArray){
                JSONArray queryArray = (JSONArray) json.get(query);
                for(Object obj : queryArray){
                    HashMap<String, List<Integer>> map = store.getOrDefault(query, new HashMap<>());
                    List<Integer> list = map.getOrDefault(obj+"", new ArrayList<>());
                    lists.add(list);
                }
            }
            else
            {
                HashMap<String, List<Integer>> map = store.getOrDefault(query, new HashMap<>());
                List<Integer> list = map.getOrDefault(json.get(query)+"", new ArrayList<>());
                lists.add(list);
            }
        }
    }

    public static void addJSON(String s, HashMap<Integer, String> documents, HashMap<String, HashMap<String, List<Integer>>> store) throws ParseException
    {
        JSONParser parser = new JSONParser();
        JSONObject json = (JSONObject) parser.parse(s);
        documents.put(docIndex, s);
        add(json, store);
        docIndex++;
    }

    public static void add(JSONObject json, HashMap<String, HashMap<String, List<Integer>>> store)
    {
        Set<String> querySet = json.keySet();
        for(String query:querySet){
            if(json.get(query) instanceof JSONObject){
                add((JSONObject)json.get(query), store);
            }
            else if(json.get(query) instanceof JSONArray){
                JSONArray queryArray = (JSONArray) json.get(query);
                for(Object obj : queryArray){
                    HashMap<String, List<Integer>> map = store.getOrDefault(query, new HashMap<>());
                    List<Integer> list = map.getOrDefault(obj+"", new ArrayList<>());
                    list.add(docIndex);
                    map.put(obj+"", list);
                    store.put(query, map);
                }
            }
            else{
                HashMap<String, List<Integer>> map = store.getOrDefault(query, new HashMap<>());
                List<Integer> list = map.getOrDefault(json.get(query)+"", new ArrayList<>());
                list.add(docIndex);
                map.put(json.get(query)+"", list);
                store.put(query, map);
            }
        }
    }

    public static List<Integer> getIntersection(List<Integer> l1, List<Integer> l2)
    {
        List<Integer> inter = new ArrayList<>();
        int i=0;
        int j=0;
        while(i<l1.size() && j<l2.size()){
            if(l1.get(i)<l2.get(j)){
                i++;
            }
            else if(l2.get(j)<l1.get(i)){
                j++;
            }
            else{
                inter.add(l1.get(i));
                i++;
                j++;
            }
        }
        return inter;
    }

}
