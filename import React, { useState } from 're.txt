import React, { useState } from 'react';
import axios from 'axios';
import '../styles/BookAppointment.css';

const BookAppointment = () => {
  const [form, setForm] = useState({
    fullName: '',
    email: '',
    contactNumber: '',
    dateOfBirth: '',
    gender: '',
    bloodType: '',
    preferredDate: '',
    timeSlot: '',
    location: '',
    notes: '',
    donorID: '', // optionally pre-filled if user is logged in
    hospitalID: '' // can be selected or assigned automatically
  });

  const handleChange = (e) => {
    setForm({ ...form, [e.target.name]: e.target.value });
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    try {
      const response = await axios.post('http://localhost:8080/appointments', {
        donorID: form.donorID,
        hospitalID: form.hospitalID,
        appointmentDate: form.preferredDate,
        status: 'Pending'
      });

      if (response.status === 200 || response.status === 201) {
        alert('Appointment submitted successfully!');
      } else {
        alert('Unexpected response from server.');
        console.log('Server response:', response);
      }

    } catch (error) {
      console.error('Error submitting appointment:', error);
      alert('There was an error submitting appointment.');
    }
  };

  return (
    <div className="book-appointment-container">
      <h2 className="form-title">🧸 Blood Donation Appointment Form</h2>
      <form onSubmit={handleSubmit} className="book-appointment-form">
        <h3>Donor Information</h3>
        <input type="text" name="fullName" placeholder="Full Name" value={form.fullName} onChange={handleChange} required />
        <input type="email" name="email" placeholder="Email Address" value={form.email} onChange={handleChange} required />
        <input type="text" name="contactNumber" placeholder="Phone Number" value={form.contactNumber} onChange={handleChange} required />
        <input type="date" name="dateOfBirth" value={form.dateOfBirth} onChange={handleChange} required />
        <select name="gender" value={form.gender} onChange={handleChange} required>
          <option value="">Select Gender</option>
          <option value="Male">Male</option>
          <option value="Female">Female</option>
          <option value="Other">Other</option>
        </select>

        <h3>Blood Donation Details</h3>
        <select name="bloodType" value={form.bloodType} onChange={handleChange} required>
          <option value="">Select Blood Type</option>
          <option value="A+">A+</option>
          <option value="A-">A-</option>
          <option value="B+">B+</option>
          <option value="B-">B-</option>
          <option value="AB+">AB+</option>
          <option value="AB-">AB-</option>
          <option value="O+">O+</option>
          <option value="O-">O-</option>
        </select>
        <input type="date" name="preferredDate" value={form.preferredDate} onChange={handleChange} required />
        <select name="timeSlot" value={form.timeSlot} onChange={handleChange} required>
          <option value="">Select Time Slot</option>
          <option value="9:00 AM - 10:00 AM">9:00 AM - 10:00 AM</option>
          <option value="10:00 AM - 11:00 AM">10:00 AM - 11:00 AM</option>
          <option value="11:00 AM - 12:00 PM">11:00 AM - 12:00 PM</option>
          <option value="1:00 PM - 2:00 PM">1:00 PM - 2:00 PM</option>
          <option value="2:00 PM - 3:00 PM">2:00 PM - 3:00 PM</option>
        </select>

        <h3>Location</h3>
        <select name="location" value={form.location} onChange={handleChange} required>
          <option value="">Select Location</option>
          <option value="Main Hospital">Main Hospital</option>
          <option value="Community Health Center">Community Health Center</option>
          <option value="Mobile Blood Drive">Mobile Blood Drive</option>
        </select>

        <textarea name="notes" placeholder="Additional Notes" value={form.notes} onChange={handleChange} />

        {/* Hidden fields or logic for authenticated users */}
        <input type="hidden" name="donorID" value={form.donorID} />
        <input type="hidden" name="hospitalID" value={form.hospitalID} />

        <button type="submit" className="btn btn-primary mt-3">Submit Appointment</button>
      </form>
    </div>
  );
};

export default BookAppointment;
