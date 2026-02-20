from fastapi import FastAPI,HTTPException,Path,Query
from fastapi.responses import JSONResponse
from pydantic import BaseModel,Field,computed_field
from typing import Annotated,Literal,Optional
import json



app=FastAPI()

#creating a pydnting fucntion for put and post and delete

class Patient(BaseModel):
    id:Annotated[str, Field(..., description="Id of the patient",examples=['P001'])]
    name:Annotated[str,Field(..., description="Name of the painets",examples=['ram'])]
    city:Annotated[str,Field(...,description='city name of the paents',examples=['mumbai'])]
    age:Annotated[int,Field(...,gt=0,lt=120,description='Age of the patient')]
    gender:Annotated[Literal['male','female','others'],Field(...,description='Gender of the pacient')]
    height:Annotated[int,Field(...,gt=0,description='height of the patients')]
    weight:Annotated[int,Field(...,gt=0,description='weight  of the patientin kgs')]


    @computed_field
    @property
    def bmi(self) -> float:
        return round(self.weight / (self.height ** 2), 2)
        return bmi
    
    @computed_field
    @property

    def verdict(self)->str:

        if self.bmi <18.5:
            return 'uderweight'
        
        elif self.bmi <25:
            return 'Normal'
        
        elif self.bmi <30:
            return 'overweight'
        
        else:
            return 'obese'
        
class PatientUpdate(BaseModel):
    name:Annotated[Optional[str],Field(default=None)]
    city:Annotated[Optional[str],Field(default=None)]
    age:Annotated[Optional[int],Field(default=None,gt=0)]
    gender:Annotated[Optional[Literal['male','female']],Field(default=None)]
    height:Annotated[Optional[float], Field(default=None,gt=0)]
    weight:Annotated[Optional[float], Field(default=None,gt=0 )]





def load_data():
    with open ("patients.json",'r') as f:
        data=json.load(f)
    return data

def save_data(data):
    with open('patients.json',"w") as f:
        json.dump(data,f)

#this is start 
@app.get("/")
def start():
    return {
        'Day':1,
        'Project summary ': 'this is my project belong to alpha 25 which is belong to fast api  and this is final project i am making for it hope to get job'
        }

#this is fopr viewing  the data 
@app.get("/view")
def view():
    data=load_data()
    return data

#this is for viewing person wise data

@app.get('/patients/{patient_id}')
def view_patient(patient_id:str = Path(..., description="this data require the number fom DB",example='P001')):
    data=load_data()

    if patient_id in data:
        return data[patient_id]
    raise HTTPException(status_code=404,detail='Item no found')

#shorting the data based on height,weright and bmi and order byin asc or desc

@app.get('/sort')
def sort_patients(sort_by:str=Query(..., description= 'sort on the basis of height,weight or bmi'), order:str=Query('asc', description='this is done in asc or desc')):

    valid_fields=['height','weight','bmi']

    if sort_by not in valid_fields:
        raise HTTPException(status_code=404,detail=f'this is not a valid ')
    
    if order not in ['asc','desc']:
        raise HTTPException(status_code=404,detail=f'you canonly use asc or desc')
    
    data=load_data()

    # sort_order=True if order=='desc' else False

    sorted_data = sorted(
        data.values(),
        key=lambda x: x.get(sort_by, 0),
        reverse=(order == "desc")
    )

    return sorted_data

#now creting a function

@app.post("/create")
def create_patients(patient:Patient):

    data=load_data()

    if patient.id in data:
        raise HTTPException(status_code=400, detail='pacient dat is already exists')
    
    data[patient.id]=patient.model_dump(exclude=['id'])

    save_data(data)

    return JSONResponse(status_code=201,content={'message':'patient created sucessfully'})


#now we a update the code usingh the put method

@app.put("/edit/{patient_id}")
def Updated_patient(patient_id:str,patient_update:PatientUpdate):

    data=load_data()

    if patient_id not in data:
        raise HTTPException(status_code=404,detail='Patient data is not found')
    
    existing_patient_info = data[patient_id]

    Updated_patient_info = patient_update.model_dump(exclude_unset=True)

    for key,value in Updated_patient_info.items():
        existing_patient_info[key]=value

    
    existing_patient_info['id']=patient_id

    patient_pydandic_obj=Patient(**existing_patient_info)

    existing_patient_info=patient_pydandic_obj.model_dump(exclude='id')


    data[patient_id]=existing_patient_info

    save_data(data)

    return JSONResponse(status_code=200,content={'message':'patient updated'})


#create a delete feature in this code 

@app.delete('/delete/{patient_id}')
def delete_patient(patient_id:str):

    #load data
    data=load_data()

    if patient_id not in data:
        raise HTTPException(status_code=404,detail="this data not availbale ")
    

    del data[patient_id]

    save_data(data)

    return JSONResponse(status_code=200, content={'message':"patient deleted"})









