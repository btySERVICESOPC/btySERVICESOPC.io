# btySERVICESOPC.io
import React, { useState } from 'react';
import { Download, Edit2, Save, X } from 'lucide-react';

const PayrollSystem = () => {
  const [view, setView] = useState('login');
  const [userType, setUserType] = useState(null);
  const [editingOffice, setEditingOffice] = useState(false);
  
  const [officeAddress, setOfficeAddress] = useState('123 Business St, Makati City, Metro Manila 1200');
  const [tempAddress, setTempAddress] = useState(officeAddress);
  
  const [employees] = useState([
    {
      id: 1,
      name: 'Juan Dela Cruz',
      email: 'juan.delacruz@company.com',
      payDate: '2024-12-15',
      paymentMethod: 'Bank Transfer',
      monthlyPay: 25000,
      payrollPeriod: 'December 1-15, 2024',
      daysWorked: 11,
      rate: 1136.36,
      standardPay: 12500,
      dailyAllowances: 1100,
      nightDifferential: 500,
      holidayPay: 1136.36,
      taxAdjustments: 0,
      serviceIncentiveLeave: 0,
      variance: 0,
      tax: 1250,
      sss: 625,
      philhealth: 312.50,
      pagibig: 100,
      sssLoan: 500,
      pagibigLoan: 0,
      absences: 0,
      internalAdjustment: 0
    },
    {
      id: 2,
      name: 'Maria Santos',
      email: 'maria.santos@company.com',
      payDate: '2024-12-15',
      paymentMethod: 'Bank Transfer',
      monthlyPay: 30000,
      payrollPeriod: 'December 1-15, 2024',
      daysWorked: 11,
      rate: 1363.64,
      standardPay: 15000,
      dailyAllowances: 1100,
      nightDifferential: 800,
      holidayPay: 1363.64,
      taxAdjustments: -200,
      serviceIncentiveLeave: 500,
      variance: 0,
      tax: 1800,
      sss: 750,
      philhealth: 375,
      pagibig: 100,
      sssLoan: 0,
      pagibigLoan: 300,
      absences: 681.82,
      internalAdjustment: 100
    }
  ]);
  
  const [selectedEmployee, setSelectedEmployee] = useState(null);

  const calculateGrossPay = (emp) => {
    return emp.standardPay + emp.dailyAllowances + emp.nightDifferential + 
           emp.holidayPay + emp.taxAdjustments + emp.serviceIncentiveLeave + emp.variance;
  };

  const calculateTotalDeductions = (emp) => {
    return emp.tax + emp.sss + emp.philhealth + emp.pagibig + emp.sssLoan + 
           emp.pagibigLoan + emp.absences + emp.internalAdjustment;
  };

  const calculateTotalPay = (emp) => {
    return calculateGrossPay(emp) - calculateTotalDeductions(emp);
  };

  const calculateTaxBased = (emp) => {
    return calculateGrossPay(emp) - emp.sss - emp.philhealth - emp.pagibig;
  };

  const handleLogin = (type) => {
    setUserType(type);
    if (type === 'employee') {
      setSelectedEmployee(employees[0]);
    }
    setView('dashboard');
  };

  const saveOfficeAddress = () => {
    setOfficeAddress(tempAddress);
    setEditingOffice(false);
  };

  const cancelEdit = () => {
    setTempAddress(officeAddress);
    setEditingOffice(false);
  };

  const formatCurrency = (amount) => {
    return new Intl.NumberFormat('en-PH', {
      style: 'currency',
      currency: 'PHP'
    }).format(amount);
  };

  const downloadPayslip = () => {
    alert('Payslip download feature would generate a PDF here');
  };

  if (view === 'login') {
    return (
      <div className="min-h-screen bg-gradient-to-br from-blue-50 to-indigo-100 flex items-center justify-center p-4">
        <div className="bg-white rounded-lg shadow-xl p-8 max-w-md w-full">
          <h1 className="text-3xl font-bold text-gray-800 mb-2 text-center">Payroll System</h1>
          <p className="text-gray-600 mb-8 text-center">Select your account type</p>
          
          <div className="space-y-4">
            <button
              onClick={() => handleLogin('employee')}
              className="w-full bg-blue-600 hover:bg-blue-700 text-white font-semibold py-4 px-6 rounded-lg transition duration-200 shadow-md"
            >
              Employee Login
            </button>
            <button
              onClick={() => handleLogin('employer')}
              className="w-full bg-indigo-600 hover:bg-indigo-700 text-white font-semibold py-4 px-6 rounded-lg transition duration-200 shadow-md"
            >
              Employer Login
            </button>
          </div>
        </div>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-gray-50 p-4">
      <div className="max-w-5xl mx-auto">
        <div className="bg-white rounded-lg shadow-lg mb-6 p-6">
          <div className="flex justify-between items-center">
            <div>
              <h1 className="text-2xl font-bold text-gray-800">
                {userType === 'employee' ? 'Employee Portal' : 'Employer Dashboard'}
              </h1>
              <p className="text-gray-600">Payroll Management System</p>
            </div>
            <button
              onClick={() => {
                setView('login');
                setUserType(null);
                setSelectedEmployee(null);
              }}
              className="bg-gray-200 hover:bg-gray-300 text-gray-700 px-4 py-2 rounded-lg transition"
            >
              Logout
            </button>
          </div>
        </div>

        {userType === 'employer' && (
          <div className="bg-white rounded-lg shadow-lg mb-6 p-6">
            <h2 className="text-xl font-bold text-gray-800 mb-4">Select Employee</h2>
            <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
              {employees.map(emp => (
                <button
                  key={emp.id}
                  onClick={() => setSelectedEmployee(emp)}
                  className={`p-4 rounded-lg border-2 transition ${
                    selectedEmployee?.id === emp.id
                      ? 'border-indigo-600 bg-indigo-50'
                      : 'border-gray-200 hover:border-indigo-300'
                  }`}
                >
                  <div className="text-left">
                    <p className="font-semibold text-gray-800">{emp.name}</p>
                    <p className="text-sm text-gray-600">{emp.email}</p>
                  </div>
                </button>
              ))}
            </div>
          </div>
        )}

        {selectedEmployee && (
          <div className="bg-white rounded-lg shadow-lg p-8">
            <div className="flex justify-between items-start mb-6">
              <h2 className="text-2xl font-bold text-gray-800">Payslip</h2>
              <button
                onClick={downloadPayslip}
                className="flex items-center gap-2 bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded-lg transition"
              >
                <Download size={18} />
                Download
              </button>
            </div>

            <div className="mb-6 pb-6 border-b">
              <div className="flex justify-between items-start mb-4">
                <div className="flex-1">
                  <p className="text-sm text-gray-600 mb-1">Office Address</p>
                  {editingOffice ? (
                    <div className="flex gap-2">
                      <input
                        type="text"
                        value={tempAddress}
                        onChange={(e) => setTempAddress(e.target.value)}
                        className="flex-1 border border-gray-300 rounded px-3 py-2"
                      />
                      <button
                        onClick={saveOfficeAddress}
                        className="bg-green-600 hover:bg-green-700 text-white p-2 rounded"
                      >
                        <Save size={18} />
                      </button>
                      <button
                        onClick={cancelEdit}
                        className="bg-gray-300 hover:bg-gray-400 text-gray-700 p-2 rounded"
                      >
                        <X size={18} />
                      </button>
                    </div>
                  ) : (
                    <div className="flex justify-between items-center">
                      <p className="font-medium text-gray-800">{officeAddress}</p>
                      {userType === 'employer' && (
                        <button
                          onClick={() => {
                            setEditingOffice(true);
                            setTempAddress(officeAddress);
                          }}
                          className="ml-4 text-blue-600 hover:text-blue-700"
                        >
                          <Edit2 size={18} />
                        </button>
                      )}
                    </div>
                  )}
                </div>
              </div>

              <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
                <div>
                  <p className="text-sm text-gray-600">Employee Name</p>
                  <p className="font-medium text-gray-800">{selectedEmployee.name}</p>
                </div>
                <div>
                  <p className="text-sm text-gray-600">Employee Email</p>
                  <p className="font-medium text-gray-800">{selectedEmployee.email}</p>
                </div>
                <div>
                  <p className="text-sm text-gray-600">Pay Date</p>
                  <p className="font-medium text-gray-800">{selectedEmployee.payDate}</p>
                </div>
                <div>
                  <p className="text-sm text-gray-600">Payment Method</p>
                  <p className="font-medium text-gray-800">{selectedEmployee.paymentMethod}</p>
                </div>
                <div>
                  <p className="text-sm text-gray-600">Total Monthly Pay</p>
                  <p className="font-medium text-gray-800">{formatCurrency(selectedEmployee.monthlyPay)}</p>
                </div>
                <div>
                  <p className="text-sm text-gray-600">Payroll Period</p>
                  <p className="font-medium text-gray-800">{selectedEmployee.payrollPeriod}</p>
                </div>
              </div>
            </div>

            <div className="mb-6">
              <h3 className="text-lg font-bold text-gray-800 mb-4 bg-blue-50 p-3 rounded">Earnings</h3>
              <div className="space-y-2">
                <div className="flex justify-between py-2 border-b">
                  <span className="text-gray-700">Days Worked</span>
                  <span className="font-medium">{selectedEmployee.daysWorked} days</span>
                </div>
                <div className="flex justify-between py-2 border-b">
                  <span className="text-gray-700">Rate</span>
                  <span className="font-medium">{formatCurrency(selectedEmployee.rate)}</span>
                </div>
                <div className="flex justify-between py-2 border-b">
                  <span className="text-gray-700">Standard Pay</span>
                  <span className="font-medium">{formatCurrency(selectedEmployee.standardPay)}</span>
                </div>
                <div className="flex justify-between py-2 border-b">
                  <span className="text-gray-700">Daily Allowances</span>
                  <span className="font-medium">{formatCurrency(selectedEmployee.dailyAllowances)}</span>
                </div>
                <div className="flex justify-between py-2 border-b">
                  <span className="text-gray-700">Night Differential</span>
                  <span className="font-medium">{formatCurrency(selectedEmployee.nightDifferential)}</span>
                </div>
                <div className="flex justify-between py-2 border-b">
                  <span className="text-gray-700">Holiday Pay</span>
                  <span className="font-medium">{formatCurrency(selectedEmployee.holidayPay)}</span>
                </div>
                <div className="flex justify-between py-2 border-b">
                  <span className="text-gray-700">Tax Adjustments</span>
                  <span className="font-medium">{formatCurrency(selectedEmployee.taxAdjustments)}</span>
                </div>
                <div className="flex justify-between py-2 border-b">
                  <span className="text-gray-700">Service Incentive Leave</span>
                  <span className="font-medium">{formatCurrency(selectedEmployee.serviceIncentiveLeave)}</span>
                </div>
                <div className="flex justify-between py-2 border-b">
                  <span className="text-gray-700">Variance</span>
                  <span className="font-medium">{formatCurrency(selectedEmployee.variance)}</span>
                </div>
                <div className="flex justify-between py-3 bg-green-50 px-3 rounded font-bold text-lg">
                  <span className="text-gray-800">Gross Pay</span>
                  <span className="text-green-700">{formatCurrency(calculateGrossPay(selectedEmployee))}</span>
                </div>
              </div>
            </div>

            <div className="mb-6">
              <h3 className="text-lg font-bold text-gray-800 mb-4 bg-red-50 p-3 rounded">Deductions</h3>
              <div className="space-y-2">
                <div className="flex justify-between py-2 border-b">
                  <span className="text-gray-700">TAX</span>
                  <span className="font-medium text-red-600">{formatCurrency(selectedEmployee.tax)}</span>
                </div>
                <div className="flex justify-between py-2 border-b">
                  <span className="text-gray-700">Social Security System (SSS)</span>
                  <span className="font-medium text-red-600">{formatCurrency(selectedEmployee.sss)}</span>
                </div>
                <div className="flex justify-between py-2 border-b">
                  <span className="text-gray-700">PhilHealth</span>
                  <span className="font-medium text-red-600">{formatCurrency(selectedEmployee.philhealth)}</span>
                </div>
                <div className="flex justify-between py-2 border-b">
                  <span className="text-gray-700">Pag-IBIG</span>
                  <span className="font-medium text-red-600">{formatCurrency(selectedEmployee.pagibig)}</span>
                </div>
                <div className="flex justify-between py-2 border-b">
                  <span className="text-gray-700">SSS Loan</span>
                  <span className="font-medium text-red-600">{formatCurrency(selectedEmployee.sssLoan)}</span>
                </div>
                <div className="flex justify-between py-2 border-b">
                  <span className="text-gray-700">Pag-IBIG Loan</span>
                  <span className="font-medium text-red-600">{formatCurrency(selectedEmployee.pagibigLoan)}</span>
                </div>
                <div className="flex justify-between py-2 border-b">
                  <span className="text-gray-700">Absences</span>
                  <span className="font-medium text-red-600">{formatCurrency(selectedEmployee.absences)}</span>
                </div>
                <div className="flex justify-between py-2 border-b">
                  <span className="text-gray-700">Internal Adjustment</span>
                  <span className="font-medium text-red-600">{formatCurrency(selectedEmployee.internalAdjustment)}</span>
                </div>
                <div className="flex justify-between py-3 bg-red-50 px-3 rounded font-bold text-lg">
                  <span className="text-gray-800">Total Deduction</span>
                  <span className="text-red-700">{formatCurrency(calculateTotalDeductions(selectedEmployee))}</span>
                </div>
              </div>
            </div>

            <div className="space-y-3">
              <div className="flex justify-between py-3 bg-blue-50 px-3 rounded font-bold">
                <span className="text-gray-800">Tax Based</span>
                <span className="text-blue-700">{formatCurrency(calculateTaxBased(selectedEmployee))}</span>
              </div>
              <div className="flex justify-between py-4 bg-gradient-to-r from-green-600 to-green-700 px-4 rounded-lg font-bold text-xl text-white shadow-lg">
                <span>Total Pay</span>
                <span>{formatCurrency(calculateTotalPay(selectedEmployee))}</span>
              </div>
            </div>
          </div>
        )}
      </div>
    </div>
  );
};

export default PayrollSystem;
