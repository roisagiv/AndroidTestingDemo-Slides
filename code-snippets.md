### Hello World

```bash
git checkout d8ebbc9
```

```base
adb shell pm list packages
```

```java
public class HelloWorldActivity extends ActionBarActivity implements View.OnClickListener {

  private EditText nameEditText;
  private TextView helloNameTextView;

  @Override protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_hello_world);

    nameEditText = (EditText) findViewById(R.id.edittext_name);
    helloNameTextView = (TextView) findViewById(R.id.textview_hello_name);
    View clickMeButton = findViewById(R.id.button_click_me);
    clickMeButton.setOnClickListener(this);
  }

  @Override public boolean onCreateOptionsMenu(Menu menu) {
    // Inflate the menu; this adds items to the action bar if it is present.
    getMenuInflater().inflate(R.menu.menu_hello_world, menu);
    return true;
  }

  @Override public boolean onOptionsItemSelected(MenuItem item) {
    // Handle action bar item clicks here. The action bar will
    // automatically handle clicks on the Home/Up button, so long
    // as you specify a parent activity in AndroidManifest.xml.
    int id = item.getItemId();

    //noinspection SimplifiableIfStatement
    if (id == R.id.action_settings) {
      return true;
    }

    return super.onOptionsItemSelected(item);
  }

  @Override public void onClick(View v) {
    String name = nameEditText.getText().toString();
    helloNameTextView.setText(String.format("Hello %s!", name));
  }
}
```


### Real world example

##### LocalFileUsersAPIClient

```bash
git checkout 3b6c15b
```

```java
public class LocalFileUsersAPIClient implements UsersAPIClient {

  private static final String FILE_NAME = "randomuser.me.10.json";

  private final AssetManager assetManager;

  public LocalFileUsersAPIClient(AssetManager assetManager) {
    this.assetManager = assetManager;
  }

  @Override public List<User> getUsers() {
    List<User> users = null;

    try {
      // read the file from assets
      InputStream inputStream = assetManager.open(FILE_NAME);
      byte[] content = new byte[inputStream.available()];
      inputStream.read(content);

      // convert string into json
      String jsonContent = new String(content);
      JSONObject root = new JSONObject(jsonContent);

      JSONArray results = root.getJSONArray("results");
      users = new ArrayList<>(results.length());

      for (int i = 0; i < results.length(); i++) {

        JSONObject userJsonObject = results.getJSONObject(i).getJSONObject("user");
        User user = getUserFromJson(userJsonObject);

        users.add(user);
      }

      //
    } catch (IOException | JSONException e) {
      e.printStackTrace();
    }

    return users;
  }

  private User getUserFromJson(JSONObject userJsonObject) throws JSONException {
    User user = new User();

    user.setEmail(userJsonObject.getString("email"));
    user.setImageUrl(userJsonObject.getJSONObject("picture").getString("medium"));

    JSONObject nameJsonObject = userJsonObject.getJSONObject("name");
    String firstName = nameJsonObject.getString("first");
    String lastName = nameJsonObject.getString("last");
    user.setName(String.format("%s %s", firstName, lastName));

    return user;
  }
}
```

##### ListUsersRecyclerViewAdapterTest - Part 1

```bash
git checkout 49b7d4e
```

```java
public class ListUsersRecyclerViewAdapter
    extends RecyclerView.Adapter<ListUsersRecyclerViewAdapter.UserViewHolder> {

  private final List<User> users;

  public ListUsersRecyclerViewAdapter() {
    users = new ArrayList<>();
  }

  public void setUsers(List<User> users) {
    this.users.clear();
    this.users.addAll(users);
  }

  /**
   *
   */
  @Override public UserViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
    View view =
        LayoutInflater.from(parent.getContext()).inflate(R.layout.list_item_user, parent, false);

    return new UserViewHolder(view);
  }

  /**
   *
   */
  @Override public void onBindViewHolder(UserViewHolder holder, int position) {

  }

  /**
   *
   */
  @Override public int getItemCount() {
    return users.size();
  }

  public static class UserViewHolder extends RecyclerView.ViewHolder {

    public UserViewHolder(View itemView) {
      super(itemView);
    }
  }
}
```

##### ListUsersRecyclerViewAdapterTest - Part 2

```bash
git checkout 17f71c5
```

```java
public class ListUsersRecyclerViewAdapter
    extends RecyclerView.Adapter<ListUsersRecyclerViewAdapter.UserViewHolder> {

  private final List<User> users;

  public ListUsersRecyclerViewAdapter() {
    users = new ArrayList<>();
  }

  public void setUsers(List<User> users) {
    this.users.clear();
    this.users.addAll(users);

    notifyDataSetChanged();
  }

  /**
   *
   */
  @Override public UserViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
    View view =
        LayoutInflater.from(parent.getContext()).inflate(R.layout.list_item_user, parent, false);

    return new UserViewHolder(view);
  }

  /**
   *
   */
  @Override public void onBindViewHolder(UserViewHolder holder, int position) {
    User user = users.get(position);

    holder.userNameTextView.setText(user.getName());
    holder.userEmailTextView.setText(user.getEmail());
  }

  /**
   *
   */
  @Override public int getItemCount() {
    return users.size();
  }

  public static class UserViewHolder extends RecyclerView.ViewHolder {

    private final TextView userNameTextView;
    private final TextView userEmailTextView;

    public UserViewHolder(View itemView) {
      super(itemView);

      userNameTextView = (TextView) itemView.findViewById(R.id.textview_user_name);
      userEmailTextView = (TextView) itemView.findViewById(R.id.textview_user_email);
    }
  }
}
```

##### ListUsersRecyclerViewAdapterTest - Part 3

```bash
git checkout 5efaa9d
```

```java
public class ListUsersRecyclerViewAdapter
    extends RecyclerView.Adapter<ListUsersRecyclerViewAdapter.UserViewHolder> {

  private final List<User> users;
  private final ImageDownloader imageDownloader;

  public ListUsersRecyclerViewAdapter(ImageDownloader imageDownloader) {
    this.imageDownloader = imageDownloader;
    users = new ArrayList<>();
  }

  public void setUsers(List<User> users) {
    this.users.clear();
    this.users.addAll(users);

    notifyDataSetChanged();
  }

  /**
   *
   */
  @Override public UserViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
    View view =
        LayoutInflater.from(parent.getContext()).inflate(R.layout.list_item_user, parent, false);

    return new UserViewHolder(view);
  }

  /**
   *
   */
  @Override public void onBindViewHolder(UserViewHolder holder, int position) {
    User user = users.get(position);

    holder.userNameTextView.setText(user.getName());
    holder.userEmailTextView.setText(user.getEmail());

    imageDownloader.setImageUrlInImageView(user.getImageUrl(), holder.userImageView);
  }

  /**
   *
   */
  @Override public int getItemCount() {
    return users.size();
  }

  public static class UserViewHolder extends RecyclerView.ViewHolder {

    private final TextView userNameTextView;
    private final TextView userEmailTextView;
    private final ImageView userImageView;

    public UserViewHolder(View itemView) {
      super(itemView);

      userNameTextView = (TextView) itemView.findViewById(R.id.textview_user_name);
      userEmailTextView = (TextView) itemView.findViewById(R.id.textview_user_email);
      userImageView = (ImageView) itemView.findViewById(R.id.imageview_user_image);
    }
  }
}
```

##### GetUsersLoaderTest

```bash
git checkout 4c8e0c1
```

```java
public class GetUsersLoader extends AsyncTaskLoader2<List<User>> {

  private final UsersAPIClient usersAPIClient;

  public GetUsersLoader(Context context, UsersAPIClient usersAPIClient) {
    super(context);
    this.usersAPIClient = usersAPIClient;
  }

  @Override public List<User> loadInBackground() {
    return usersAPIClient.getUsers();
  }
}
```

##### UsersListActivityEspressoTest

```bash
git checkout 250a3ee
```

```java
public class UsersListActivity extends ActionBarActivity
    implements LoaderManager.LoaderCallbacks<List<User>> {

  private ListUsersRecyclerViewAdapter listUsersRecyclerViewAdapter;

  @Override protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_users_list);

    RecyclerView usersRecyclerView = (RecyclerView) findViewById(R.id.recyclerview_users);
    usersRecyclerView.setLayoutManager(new LinearLayoutManager(this));

    listUsersRecyclerViewAdapter = new ListUsersRecyclerViewAdapter(new PicassoImageDownloader());
    usersRecyclerView.setAdapter(listUsersRecyclerViewAdapter);

    getSupportLoaderManager().initLoader(0, null, this);
  }

  @Override public Loader<List<User>> onCreateLoader(int id, Bundle args) {
    return new GetUsersLoader(this, new LocalFileUsersAPIClient(getAssets()));
  }

  @Override public void onLoadFinished(Loader<List<User>> loader, List<User> data) {
    listUsersRecyclerViewAdapter.setUsers(data);
  }

  @Override public void onLoaderReset(Loader<List<User>> loader) {

  }
}
```

##### MockWebServer

```
git checkout ef695da
```
