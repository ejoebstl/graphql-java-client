# Simple Java GraphQL client

Capable of connecting to an endpoint, then fetching query results. Also accepts a mapper function to turn JSON to Java objects. 

### Usage

```java
URL graphqlEndpoint = new URL("your endpoint"); 

Map<String, String> headers = new HashMap<>();
headers.put("Authentication", "Bearer: 123456");

String query = 
    "query(\n" +
    "  searchText: String!\n" +
    ")\n" +
    "  allNotes(filter: { text_contains: $searchText } {\n" +
    "    id\n" +
    "    text\n" +
    "  }\n" +
    "}\n";

GraphQLClient client = new GraphQLClient(graphqlEndpoint, headers);

JsonObject variables = new JsonObject();
variables.addProperty("searchText", "test");

ArrayList<Note> notes = client.execute(query, variables, (JsonObject o) -> {
    ArrayList<Note> notes = new ArrayList<>();
    for(JsonElement elem : get("allNotes").getAsJsonArray()) {
        notes.add(new Note(elem.get("text).getAsString()));
    }
    return notes;
});

for(Note note : notes) {
    System.out.println(note);
}

```
