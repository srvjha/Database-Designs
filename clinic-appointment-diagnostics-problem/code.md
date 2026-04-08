```
PATIENT[icon:user-check, color: Red]{
  id (PK)
  name VARCHAR(45) NOT NULL
  phone VARCHAR(20)
  email VARCHAR(100)
  date_of_birth DATE
  gender ENUM("MALE", "FEMALE")
  address TEXT
  created_at TIMESTAMP
  updated_at TIMESTAMP
}

DOCTOR[icon:user-lock, color: Blue]{
  id (PK)
  name VARCHAR(55) NOT NULL
  specialty TEXT
  experience_years INT
  created_at TIMESTAMP
}

APPOINTMENT[icon:calendar-check, color: Yellow]{
  id (PK)
  patient_id (FK)
  doctor_id (FK)
  scheduled_at TIMESTAMP
  status ENUM ("SCHEDULED", "CANCELLED", "COMPLETED", "NO_SHOW")
  reason TEXT
  notes TEXT
  rescheduled_from (FK)
  created_at TIMESTAMP
}

CONSULTATION[icon:file-text, color: Green]{
  id (PK)
  appointment_id (FK)
  patient_id (FK)
  doctor_id (FK)
  consultation_time TIMESTAMP
  diagnosis TEXT
  prescription_notes TEXT
  follow_up_date DATE
  notes TEXT
  created_at TIMESTAMP
}

TEST[icon:flask, color: Purple]{
  id (PK)
  name VARCHAR(100)
  description TEXT
  price DECIMAL
}

CONSULTATION_TEST[icon:link]{
  id (PK)
  consultation_id (FK)
  test_id (FK)
  status ENUM("PRESCRIBED", "COMPLETED")
  instructions TEXT
  scheduled_at TIMESTAMP
  completed_at TIMESTAMP
}

REPORT[icon:file, color: Red]{
  id (PK)
  consultation_test_id (FK)
  file_url TEXT
  result_summary TEXT
  status ENUM("PENDING", "READY")
  generated_at TIMESTAMP
}

PAYMENT[icon:credit-card, color: Purple]{
  id (PK)
  patient_id (FK)
  consultation_id (FK)
  appointment_id (FK)
  amount DECIMAL
  status ENUM("PENDING", "PAID", "FAILED")
  payment_method VARCHAR(50)
  paid_at TIMESTAMP
  created_at TIMESTAMP
}


PATIENT < APPOINTMENT
PATIENT < CONSULTATION
PATIENT < PAYMENT

DOCTOR < APPOINTMENT
DOCTOR < CONSULTATION


APPOINTMENT - CONSULTATION

CONSULTATION > CONSULTATION_TEST
TEST > CONSULTATION_TEST

CONSULTATION_TEST - REPORT

CONSULTATION < PAYMENT
APPOINTMENT < PAYMENT

```