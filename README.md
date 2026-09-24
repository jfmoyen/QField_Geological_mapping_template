# QField_Geological_mapping_template
A QGIS template project for geological mapping using QField

QField is a portable version of QGIS. I am using it for geological mapping, using a suite of tools to tweak the functionalities to the specific needs of this activity:
- A QGIs project and database template (this repo);
- A plugin adding a geological compass to measure and record planes and lines (here);
- A plugin streamlining the use of camera (there)

  The three tools can be used independently. Together, they offer a platform that works well (at least, for me) in the field. A source of inspiration for this project is the FieldMove application: the first goal was to reproduce FieldMove's functionalities, but adding the flexibility and "openness" of QField.

  # A database and a QGIS project for geological mapping

  This project provides presets to help recording field data. It taps into several lesser known functionalities of QGIS, such as customized forms, joins and advanced symbology to interact with a field database. Of course, the files supplied here are only a template - you should customize them according to your own needs. 

  ## The database
  The database, stored as a geopackage, which ensures better data integrity, and allows to move everything as one single file. Historically, the table fields are inherited from FieldMove (and largely remain compatible with FM's tables), which is why they have a series of probably unneeded fields.
  
   The database is made of 6 tables:
  - **Localities**
  
| Field name    | Type | Comments |
| -------- | ------- | ------- |
| January  | $250    | |
| February | $80     | |
| March    | $420    | |
