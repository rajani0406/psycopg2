1. These are all different adabpter version files for psycopg2 which is used to create a lambda layer to connect to erverless redshift or any other database
2. Just download the zip and unzip it again zip the particulat version
3. Upload it in the lambda function or lambda layer
   Here is the Lamda Connection Code   

#Connection check:
#Connection established successfully.
#finish
#This is working code with psycopg2 dataadapter 

import json
import psycopg2
 
def lambda_handler(event, context):
    conf = {
        'dbname': 'dev',
        'host': 'your host url',
        'port': '5439',
        'user': 'user',
        'password': 'pass'
    }
 
    def create_conn(*args, **kwargs):
        config = kwargs['config']
        try:
            conn = psycopg2.connect(
                dbname=config['dbname'],
                host=config['host'],
                port=config['port'],
                user=config['user'],
                password=config['password']
            )
            return conn
        except Exception as err:
            print("Connection Error:", err)
            # Raise an exception or handle the error accordingly
 
    def check_connection(*args, **kwargs):
        conn = kwargs['conn']
        try:
            # Use conn to perform any check, or just check if it's not None
            if conn:
                print("Connection established successfully.")
            else:
                print("Connection is None. Unable to establish a connection.")
        except Exception as err:
            print("Error while checking connection:", err)
 
    print('start')
    configuration = conf
    conn = create_conn(config=configuration)
    print('Connection check:')
    check_connection(conn=conn)
    print('finish')




#Query the data

import json
import psycopg2
def lambda_handler(event, context):
    conf = {
        'dbname': 'dev',
        'host': 'Your host url',
        'port': '5439',
        'user': 'dsg_admin1234',
        'password': 'hjhsdmin#202386457'
    }

    def create_conn(*args, **kwargs):
        config = kwargs['config']
        try:
            conn = psycopg2.connect(
                dbname=config['dbname'],
                host=config['host'],
                port=config['port'],
                user=config['user'],
                password=config['password']
            )
            return conn
        except Exception as err:
            print("Connection Error:", err)
            # Raise an exception or handle the error accordingly

    def check_connection(*args, **kwargs):
        conn = kwargs['conn']
        try:
            # Use conn to perform any check, or just check if it's not None
            if conn:
                print("Connection established successfully.")
            else:
                print("Connection is None. Unable to establish a connection.")
        except Exception as err:
            print("Error while checking connection:", err)

    def execute_select_query(conn, query):
        try:
            cursor = conn.cursor()
            cursor.execute(query)
            rows = cursor.fetchall()
            for row in rows:
                print(row)
        except Exception as err:
            print("Error executing SELECT query:", err)
        finally:
            cursor.close()

    print('start')
    configuration = conf
    conn = create_conn(config=configuration)
    print('Connection check:')
    check_connection(conn=conn)

    # Example SELECT query, replace 'your_table' and 'your_column' with actual names
    select_query = "SELECT id FROM dev.public.vehicle LIMIT 2;"

    # Execute the SELECT query
    execute_select_query(conn, select_query)

    print('finish')

    # Close the connection when you're done
    conn.close()
