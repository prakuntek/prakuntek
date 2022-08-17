const Joi = require('joi');
const express = require ('express');
const app = express();

app.use(express.json());

const courses = [
    {id:1, name:'course1'},
    {id:2, name:'course2'},
    {id:3, name:'course3'},


];

app.get('/', (req,res) =>{
    res.send('hello world');
});

app.get('/api/courses',(req,res) =>{
    res.send(courses); 
});
app.post('/api/courses',(req, res) => { // app.post()
    res.send(courses);

});

app.post('/api/courses', (req, res) => {
    const schema ={
        name: Joi.string().min(3).required()

    };

    const result = Joi.Validate(req.body, schema);

    if (result.error) {
        //400 bad Request
        res.status(400).send(result.error.details[0].message);
        return;
    }
    
    const course ={
        id: courses.length +1,
        name: req.body.name
    };
    courses.push(course);
    res.send(course);
});

// app.put()

app.put('/api/courses/:id', (req, res) => {

    const course =  courses.find(c => c.id === parseInt(req.params.id));
    if (!course) res.status(404).send('The course with the given ID was not found');

    //Look up the course
    // if not existing, return 404


    //Validate
    // If invalid, return 400 - bad Request
    const schema ={
        name: Joi.string().min(3).required()

    };

    const result = ValidateCourse(req.body);
    const { error} = ValidateCourse(req.body); // result.error

    if (error) {
        //400 bad Request
        res.status(400).send(result.error.details[0].message);
        return;
    }

    
    //update course
    course.name = req.body.name;
    res.send(course);
    // Return the updated course
});

function ValidateCourse(course) {
    const schema ={
        name: Joi.string().min(3).required()

    };

    return Joi.Validate(course, schema);
}





app.get('/api/courses/:id',(req,res) => {
 const course =  courses.find(c => c.id === parseInt(req.params.id));
 if (!course) res.status(404).send('The course with the given ID was not found');
 res.send(course);
});


// PORT 
const port = process.env.PORT || 3000;
app.listen(port, () => console.log (`listening on port ${port}...`));

// app.delete()
