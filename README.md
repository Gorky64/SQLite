# SQLite
An example of SQLite usage.

```java
public class MainActivity extends AppCompatActivity {
    SQLiteDatabase db;
    Button btnAdd;
    Button btnDel;
    Button btnUpdate;
    Button btnView;
    Button btnViewAll;
    EditText etID;
    EditText etIme;
    EditText etPrezime;
    EditText etAdresa;


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        btnAdd = (Button) findViewById(R.id.btnAdd);
        btnDel = (Button) findViewById(R.id.btnDel);
        btnUpdate = (Button) findViewById(R.id.btnUpdate);
        btnView = (Button) findViewById(R.id.btnView);
        btnViewAll = (Button) findViewById(R.id.btnViewAll);
        etID = (EditText) findViewById(R.id.etID);
        etIme = (EditText) findViewById(R.id.etIme);
        etPrezime = (EditText) findViewById(R.id.etPrezime);
        etAdresa = (EditText) findViewById(R.id.etAdresa);

        try {
            db = openOrCreateDatabase("StudentDB", Context.MODE_PRIVATE, null);
            db.execSQL("CREATE TABLE IF NOT EXISTS student(ID VARCHAR,Ime VARCHAR,Prezime VARCHAR, Adresa VARCHAR);");


            btnAdd.setOnClickListener(new View.OnClickListener() {
                public void onClick(View v) {

                    String sError = etID.getText().toString();
                    if (sError.equals(""))

                        showMessage("Error", "ID needed");
                    else
                        db.execSQL("INSERT INTO student VALUES('" + etID.getText() + "','" + etIme.getText() + "','" + etPrezime.getText() + "','" + etAdresa.getText() + "');");


                    etID.setText("");
                    etIme.setText("");
                    etPrezime.setText("");
                    etAdresa.setText("");

                }
            });

            btnDel.setOnClickListener(new View.OnClickListener() {
                public void onClick(View v) {
                    String sError = etID.getText().toString();
                    if (sError.equals(""))

                        showMessage("Error", "ID needed");

                    Cursor c = db.rawQuery("SELECT * FROM student WHERE ID='" + etID.getText() + "'", null);
                    if (c.moveToFirst()) {
                        db.execSQL("DELETE FROM student WHERE ID='" + etID.getText() + "'");
                        etID.setText("");
                        etIme.setText("");
                        etPrezime.setText("");
                        etAdresa.setText("");
                        showMessage("Success", "Record Deleted");

                    }
                }
            });

            btnUpdate.setOnClickListener(new View.OnClickListener() {
                public void onClick(View v) {

                    Cursor c = db.rawQuery("SELECT * FROM student WHERE ID='" + etID.getText() + "'", null);
                    if (c.moveToFirst()) {
                        db.execSQL("UPDATE student SET Ime='" + etIme.getText() + "',Prezime='" + etPrezime.getText() + "',Adresa='" + etAdresa.getText() +
                                "'WHERE ID ='" + etID.getText() + "'");
                        etID.setText("");
                        etIme.setText("");
                        etPrezime.setText("");
                        etAdresa.setText("");

                        showMessage("Success", "Record Updated");
                    }
                }
            });

            btnView.setOnClickListener(new View.OnClickListener() {
                public void onClick(View v) {
                    Cursor c = db.rawQuery("SELECT * FROM student WHERE ID ='" + etID.getText() + "'", null);
                    if (c.moveToFirst()) {
                        etIme.setText(c.getString(1));
                        etPrezime.setText(c.getString(2));
                        etAdresa.setText(c.getString(3));
                    }

                }
            });


            btnViewAll.setOnClickListener(new View.OnClickListener() {
                public void onClick(View v) {

                    Cursor c = db.rawQuery("SELECT * FROM student", null);
                    if (c.getCount() == 0) {
                        showMessage("Error", "No records found");
                        return;
                    }
                    StringBuffer buffer = new StringBuffer();
                    while (c.moveToNext()) {
                        buffer.append("ID : " + c.getString(0) + "\n");
                        buffer.append("Ime : " + c.getString(1) + "\n");
                        buffer.append("Prezime : " + c.getString(2) + "\n");
                        buffer.append("Adresa : " + c.getString(3) + "\n");
                    }
                    showMessage("Student Details", buffer.toString());
                }
            });
        } catch (Exception e) {
            showMessage("", e.toString());
        }
    }

    public void showMessage(String title, String message) {
        Builder builder = new Builder(this);
        builder.setCancelable(true);
        builder.setTitle(title);
        builder.setMessage(message);
        builder.show();
    }
}
```
