// --- CONTRIBUTOR: SOBAN RASHEED (QUEUE WIZARD) ---

// Function 1: Add Patient to Waiting Queue
FUNCTION EnqueuePatient(patientID, name)
    CREATE new node named NewPatient
    SET NewPatient.ID = patientID
    SET NewPatient.Name = name
    SET NewPatient.Next = NULL

    IF Rear IS NULL THEN
        SET Front = NewPatient
        SET Rear = NewPatient
    ELSE
        SET Rear.Next = NewPatient
        SET Rear = NewPatient
    END IF
    PRINT "Patient added to waiting line."
END FUNCTION

// Function 2: Check if any beds are available
FUNCTION CheckAvailability()
    SET Current = HeadBed
    WHILE Current IS NOT NULL
        IF Current.Status EQUALS "Empty" THEN
            RETURN True
        END IF
        SET Current = Current.Next
    END WHILE
    RETURN False
END FUNCTION
// Function: Assign Bed (Admit Patient)
FUNCTION AssignBed(patientID, name, disease)
SET CurrentBed = HeadBedNode
// Traverse the list to find an empty bed
WHILE CurrentBed IS NOT NULL
IF CurrentBed.Status EQUALS "Empty" THEN
// Found an empty bed, assign data
SET CurrentBed.PatientID = patientID
SET CurrentBed.PatientName = name
SET CurrentBed.Disease = disease
SET CurrentBed.Status = "Occupied"
PRINT "Patient assigned to Bed Number: " + CurrentBed.BedNumber
RETURN True
END IF
SET CurrentBed = CurrentBed.Next
END WHILE
// If loop finishes without returning, no beds are free
PRINT "No beds available. Redirecting to Waiting Queue..."
RETURN False
END FUNCTION
// Function: Discharge Patient
FUNCTION DischargePatient(bedNumber)
SET CurrentBed = HeadBedNode
// Find the bed to discharge
WHILE CurrentBed IS NOT NULL
IF CurrentBed.BedNumber EQUALS bedNumber THEN
PRINT "Discharging patient: " + CurrentBed.PatientName
// Clear the bed data
SET CurrentBed.PatientID = NULL
SET CurrentBed.PatientName = NULL
SET CurrentBed.Status = "Empty"
RETURN
END IF
SET CurrentBed = CurrentBed.Next
END WHILE
PRINT "Bed Number not found."
END FUNCTION
// Function: Display All Beds
FUNCTION DisplayWard()
SET CurrentBed = HeadBedNode
PRINT "--- Hospital Ward Status ---"
WHILE CurrentBed IS NOT NULL
PRINT "Bed: " + CurrentBed.BedNumber
IF CurrentBed.Status EQUALS "Occupied" THEN
PRINT " Patient: " + CurrentBed.PatientName
ELSE
PRINT " [Available]"
END IF
SET CurrentBed = CurrentBed.Next
END WHILE
PRINT "----------------------------"

END FUNCTION
