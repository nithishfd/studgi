    let employees = JSON.parse(localStorage.getItem("employees")) || [];
let empCode = employees.length > 0 
    ? employees[employees.length - 1].code + 1 
    : 100101;

     let editIndex = -1; 


function CREATE() {
 document.getElementById("overlay").style.display = "block";
 document.getElementById("modal").style.display = "block";
 document.getElementById("create").addEventListener("click", function (event) {
    event.preventDefault();   // prevents page reload
    addData();
});

 
}
function UPDATE() {
 document.getElementById("overlay_udt").style.display = "block";
 document.getElementById("modal_udt").style.display = "block";
 document.getElementById("update").addEventListener("click", function (event) {
    event.preventDefault();   // prevents page reload
    addData_udt();
});
}
function DELETE() {
 document.getElementById("overlay_dele").style.display = "block";
 document.getElementById("modal_dele").style.display = "block";
 document.getElementById("delete").addEventListener("click", function (event) {
    event.preventDefault();   // prevents page reload
    addData_dele();
});
}



function closePopup() {
document.getElementById("overlay").style.display = "none";
 document.getElementById("modal").style.display = "none";
}

function closePopup_udt() {
document.getElementById("overlay_udt").style.display = "none";
 document.getElementById("modal_udt").style.display = "none";
}

function closePopup_dele() {
document.getElementById("overlay_dele").style.display = "none";
 document.getElementById("modal_dele").style.display = "none";
}


  function openForm() {
        document.getElementById("popupForm").style.display = "block";
    }

    function closeForm() {
        document.getElementById("popupForm").style.display = "none";
    }
//let empCode = 100101;

//let employees = []; // array containing employees

function displayData() {
    let tableHTML = "";

    employees.forEach((emp, index) => {
        tableHTML += `
            <tr>
                <td>${emp.code}</td>
                <td>${emp.name}</td>
                <td>${emp.email}</td>
                <td>${emp.manager}</td>
                <td>${emp.project}</td>
                <td>
                    <i  class="fa fa-eye" onclick="viewRow(${index})" style="color: green;"></i>
                    <i class="fa fa-edit" onclick="editRow(${index})" style="color: violet;"></i>
                    <i class="fa fa-trash" onclick="deleteRow(${index})"style="color: red;"></i>
                </td>
            </tr>
        `;
    });

    document.getElementById("tbody").innerHTML = tableHTML;  // <-- Add table rows to body
}


function addData() {
    let newEmployee = {
        code: empCode,
        name: document.getElementById("empName").value,
        email: document.getElementById("empEmail").value,
        manager: document.getElementById("managname").value,
        project: document.getElementById("curProj").value
    };

    if (editIndex === -1) {
        employees.push(newEmployee);
        empCode++;   // increase for next employee

    } else {
        employees[editIndex] = newEmployee;
        editIndex = -1;
        document.getElementById("saveBtn").innerText = "Create";
    }
    
        

        localStorage.setItem("employees", JSON.stringify(employees));



  
   displayData(); 
   
    
    closeForm();
    document.getElementById("empForm").reset();
}


function editRow(index) {
    alert("Editing row " + index);

    editIndex = index;
    let emp = employees[index];

    document.getElementById("empCode").value = emp.code;
    document.getElementById("empName").value = emp.name;
    document.getElementById("empEmail").value = emp.email;
    document.getElementById("managname").value = emp.manager;
    document.getElementById("curProj").value = emp.project;

    document.getElementById("saveBtn").innerText = "Update";

    // 👇 THIS MUST COME AFTER THE ALERT
    openForm(); // show the popup form
}



function viewRow(i) {
    alert("Viewing Employee\n\n" +
        "Code: " + employees[i].code +
          "\nName: " + employees[i].name +
          "\nEmail: " + employees[i].email +
        "\nManager Name: " + employees[i].manager +
    "\nCurrent Project Name: " + employees[i].project );
}

function editRow(i) {
    alert("Editing row " + (i + 1));
}


function deleteByCode() {
    let input = document.getElementById("deleteInput").value.trim();

    if (input === "") {
        alert("Please enter Employee Code or Name");
        return;
    }

    let index = employees.findIndex(emp =>
        emp.code.toString() === input ||
        emp.name.toLowerCase() === input.toLowerCase()
    );

    if (index === -1) {
        alert("Employee not found!");
        return;
    }

    if (confirm("Are you sure you want to delete this employee?")) {
        employees.splice(index, 1);
        localStorage.setItem("employees", JSON.stringify(employees));  // <-- ADD THIS
        displayData();
        alert("Employee deleted successfully!");
        closePopup_dele();
    }
}

function deleteRow(index) {
    if (confirm("Are you sure to delete this employee?")) {

        employees.splice(index, 1);    // Remove employee
        localStorage.setItem("employees", JSON.stringify(employees));  // Save updated list

        displayData();  // Refresh table
    }
}


function getEmployeedetails() {
   let code = document.getElementById("updateCode").value.trim();

    if (code === "") {
        alert("Please enter an employee code");
        return;
    }

    // Convert to number
    code = parseInt(code);

    // Find employee by code
    let emp = employees.find(e => e.code === code);

    if (!emp) {
        alert("Employee not found!");
        return;
    }

    // Fill employee data into form fields
    document.getElementById("empName").value = emp.name;
    document.getElementById("empEmail").value = emp.email;
    document.getElementById("managname").value = emp.manager;
    document.getElementById("curProj").value = emp.project;

    alert("Employee Details Loaded Successfully!");
}




function closeForm() {
     document.getElementById("overlay").style.display = "none";
 document.getElementById("modal").style.display = "none";
}
window.onload = function () {
    displayData();
};

window.onload = displayData;

 

