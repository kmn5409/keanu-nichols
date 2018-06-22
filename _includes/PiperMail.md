
{% highlight python linenos %}
table_names = s.inspect(connect.db).get_table_names()
print(table_names)
val = False
mail_lists = True
if("mail_lists" not in table_names):
    mail_lists = False
if("mailing_list_jobs" in table_names):
    lists_createdSQL = s.sql.text("""SELECT project FROM mailing_list_jobs""")
    df1 = pd.read_sql(lists_createdSQL, connect.db)
    print(df1)
    val = True
else:
    columns2 = "backend_name","mailing_list_url","project","last_message_date"
    df_mail_list = pd.DataFrame(columns=columns2)
    df_mail_list.to_sql(name='mailing_list_jobs',con=connect.db,if_exists='replace',index=False)
    lists_createdSQL = s.sql.text("""SELECT project FROM mailing_list_jobs""")
    df1 = pd.read_sql(lists_createdSQL, connect.db)
    print("Created Table")
{% endhighlight %}

    ['issue_response_time']
    Created Table

