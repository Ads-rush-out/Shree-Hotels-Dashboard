**New Columns:**

Total Occupied Rooms = CALCULATE(COUNTROWS('Hotel Data'),'Hotel Data'[BookingStatus]="Checked-In")

Total Rooms Booked = CALCULATE(COUNTROWS('Hotel Data'))

Occupancy Rate = [Total Occupied Rooms]/[Total Rooms Booked]

Date Table = CALENDAR(MIN('Hotel Data'[CheckInDate]),MAX('Hotel Data'[CheckInDate]))

Month = FORMAT('Date Table'[Date],"mmmm")

Month Number = MONTH('Date Table'[Date])

Quarter = QUARTER('Date Table'[Date])

Quarter Name = CONCATENATE("Q",'Date Table'[Quarter])

Year = Year('Date Table'[Date])

Week = WEEKNUM('Date Table'[Date])

Total Days of Booking = CALCULATE(SUM('Hotel Data'[Duration]),'Hotel Data'[BookingStatus]="Checked-In")

Total Weekend Nights = CALCULATE(SUM('Hotel Data'[Stays In Weekend Nights]),'Hotel Data'[BookingStatus]="Checked-In")

Average Daily Rate (ADR) = CALCULATE(AVERAGE('Hotel Data'[ADR]))

Total Weekday Nights = CALCULATE(SUM('Hotel Data'[Stays In Week Nights]),'Hotel Data'[BookingStatus]="Checked-In")

RevPAR = [Average Daily Rate (ADR)]*[Occupancy Rate]

Total Cancelled Bookings = CALCULATE(COUNTROWS('Hotel Data'),'Hotel Data'[BookingStatus]="Cancelled")

Total Revenue = CALCULATE(SUM('Hotel Data'[Rev]),'Hotel Data'[BookingStatus]="Checked-In")

Week Night Revenue = 'Hotel Data'[Week Nights] * 'Hotel Data'[ADR]

Weekend Night Revenue = 'Hotel Data'[Weekend Nights] * 'Hotel Data'[ADR]

Week Night Rev = CALCULATE(SUM('Hotel Data'[Week Night Revenue]),'Hotel Data'[BookingStatus]="Checked-In")

Weekend Night Rev = CALCULATE(SUM('Hotel Data'[Weekend Night Revenue]),'Hotel Data'[BookingStatus]="Checked-In")

Average Week Revenue = CALCULATE(AVERAGEX(VALUES('Data Table'[Week]),CALCULATE(SUM('Hotel Data'[REV]))),ALL('Date Table'))

Average Week Occupied Rooms = CALCULATE(AVERAGEX(VALUES('Data Table'[Week]),CALCULATE([Total Occupied Rooms])),ALL('Date Table'))

Average Day of Stay = AVERAGE('Hotel Data'[Duration]) 
