# Sqlite

- db
```java
sampleDB = this.openOrCreateDatabase(dbName, MODE_PRIVATE, null);

            // 테이블이 존재하지 않으면 새로 생성
            sampleDB.execSQL("CREATE TABLE IF NOT EXISTS " + tableName
                    + " (name VARCHAR(20), phone VARCHAR(20) );");

            // 테이블이 존재하는 경우 기존 데이터를 지우기 위해서 사용
            sampleDB.execSQL("DELETE FROM " + tableName  );


            // 새로운 데이터를 테이블에 집어넣음
            for (int i=0; i<name.length; i++ ) {
                sampleDB.execSQL("INSERT INTO " + tableName
                        + " (name, phone)  Values ('" + name[i] + "', '" + phone[i]+"');");
            }
            sampleDB.close();

```

- 데이터 보여주기
```java
protected void showList(){

        try {
            SQLiteDatabase ReadDB = this.openOrCreateDatabase(dbName, MODE_PRIVATE, null);
            // SELECT문을 사용하여 테이블에 있는 데이터를 가져옴
            Cursor c = ReadDB.rawQuery("SELECT * FROM " + tableName, null);

            if (c != null) {
                if (c.moveToFirst()) {
                    do {
                        // 테이블에서 두개의 컬럼값을 가져와서
                        String Name = c.getString(c.getColumnIndex("name"));
                        String Phone = c.getString(c.getColumnIndex("phone"));
                        // HashMap에 넣기
                        HashMap<String,String> persons = new HashMap<String,String>();

                        persons.put(TAG_NAME,Name);
                        persons.put(TAG_PHONE,Phone);

                        // ArrayList에 추가
                        personList.add(persons);

                    } while (c.moveToNext());
                }
            }
            ReadDB.close();
            // 새로운 apapter를 생성하여 데이터를 넣은 후
            adapter = new SimpleAdapter(
                    this, personList, R.layout.list_item,
                    new String[]{TAG_NAME,TAG_PHONE},
                    new int[]{ R.id.name, R.id.phone}
            );
            // 화면에 보여주기 위해 Listview에 연결
            listView.setAdapter(adapter);
        } catch (SQLiteException se) {
            Toast.makeText(getApplicationContext(),  se.getMessage(), Toast.LENGTH_LONG).show();
            Log.e("",  se.getMessage());
        }
    }
```
