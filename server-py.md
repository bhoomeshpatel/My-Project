import email
import re
from unittest import result
import pyodbc
import json

from flask import Flask, redirect, url_for, render_template, request
app = Flask(__name__)

# @app.route('/success/<name>')
# def success(name):
#    return 'welcome %s' % name

data=[]

@app.route('/xyz',methods = ['POST', 'GET'])
def login():
   if request.method == 'POST':
      email = request.form['nm']
      print (email)
      
      server = 'xyzz.database.windows.net' 
      database = 'myDB' 
      username = 'test' 
      password = 'Admin@123' 
      cnxn = pyodbc.connect('DRIVER={ODBC Driver 17 for SQL Server};SERVER='+server+';DATABASE='+database+';UID='+username+';PWD='+ password)
      cursor = cnxn.cursor()
      cursor.execute('''
                     INSERT INTO dbo.mailID(email)
                     VALUES (?)
                     ''',(email))
      cursor.commit()
      cursor.execute('SELECT * FROM mailID')
      result =cursor.fetchall()
      for row in result:
         data.append(list(row))
      return render_template("index.html",output=data)

      # for i in data:
      #    for d in i:
      #       print(d)
      #       return d
      
  
if __name__ == '__main__':
   app.run(debug = True)
