# 131231221

import 'package:flutter/material.dart';
import 'package:table_calendar/table_calendar.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: LoginScreen(),
    );
  }
}

// 로그인 화면
class LoginScreen extends StatefulWidget {
  @override
  _LoginScreenState createState() => _LoginScreenState();
}

class _LoginScreenState extends State<LoginScreen> {
  final TextEditingController _idController = TextEditingController();
  final TextEditingController _passwordController = TextEditingController();

  @override
  void dispose() {
    _idController.dispose();
    _passwordController.dispose();
    super.dispose();
  }

  void _login() {
    final id = _idController.text.trim();
    final password = _passwordController.text.trim();

    if (id == '1234' && password == '1234') {
      // 로그인 성공 시 좌표 이동 페이지로 이동
      Navigator.push(
        context,
        MaterialPageRoute(builder: (context) => CoordinatePage()),
      );
    } else {
      // 로그인 실패 메시지
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('로그인 실패')),
      );
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.orange,
      body: Center(
        child: Padding(
          padding: const EdgeInsets.symmetric(horizontal: 24.0),
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              TextField(
                controller: _idController,
                decoration: InputDecoration(
                  filled: true,
                  fillColor: Colors.white,
                  hintText: '아이디',
                  border: OutlineInputBorder(
                    borderRadius: BorderRadius.circular(8.0),
                    borderSide: BorderSide.none,
                  ),
                  contentPadding: EdgeInsets.symmetric(horizontal: 16.0, vertical: 12.0),
                ),
              ),
              SizedBox(height: 16.0),
              TextField(
                controller: _passwordController,
                obscureText: true,
                decoration: InputDecoration(
                  filled: true,
                  fillColor: Colors.white,
                  hintText: '비밀번호',
                  border: OutlineInputBorder(
                    borderRadius: BorderRadius.circular(8.0),
                    borderSide: BorderSide.none,
                  ),
                  contentPadding: EdgeInsets.symmetric(horizontal: 16.0, vertical: 12.0),
                ),
              ),
              SizedBox(height: 24.0),
              SizedBox(
                width: double.infinity,
                child: ElevatedButton(
                  onPressed: _login,
                  style: ElevatedButton.styleFrom(
                    backgroundColor: Colors.white,
                    foregroundColor: Colors.orange,
                    padding: EdgeInsets.symmetric(vertical: 14.0),
                    shape: RoundedRectangleBorder(
                      borderRadius: BorderRadius.circular(8.0),
                    ),
                  ),
                  child: Text(
                    '로그인',
                    style: TextStyle(
                      fontSize: 16.0,
                      fontWeight: FontWeight.bold,
                    ),
                  ),
                ),
              ),
            ],
          ),
        ),
      ),
    );
  }
}

// 좌표 이동 페이지
class CoordinatePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Container(
        color: Colors.orange,
        padding: EdgeInsets.all(16.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.spaceAround,
          children: [
            ElevatedButton(
              onPressed: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(builder: (context) => DateTimeSelectionPage()),
                );
              },
              child: Text('외출'),
            ),
            ElevatedButton(
              onPressed: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(builder: (context) => DateTimeSelectionPage()),
                );
              },
              child: Text('외박'),
            ),
          ],
        ),
      ),
    );
  }
}

// 날짜 및 시간 선택 페이지
class DateTimeSelectionPage extends StatefulWidget {
  @override
  _DateTimeSelectionPageState createState() => _DateTimeSelectionPageState();
}

class _DateTimeSelectionPageState extends State<DateTimeSelectionPage> {
  DateTime _selectedDate = DateTime.now();
  TimeOfDay _selectedTime = TimeOfDay.now();
  String _confirmationMessage = '';

  Future<void> _selectDate(BuildContext context) async {
    final DateTime? picked = await showDatePicker(
      context: context,
      initialDate: _selectedDate,
      firstDate: DateTime(2000),
      lastDate: DateTime(2101),
    );
    if (picked != null && picked != _selectedDate) {
      setState(() {
        _selectedDate = picked;
      });
    }
  }

  Future<void> _selectTime(BuildContext context) async {
    final TimeOfDay? picked = await showTimePicker(
      context: context,
      initialTime: _selectedTime,
    );
    if (picked != null && picked != _selectedTime) {
      setState(() {
        _selectedTime = picked;
      });
    }
  }

  void _confirmSelection() {
    setState(() {
      _confirmationMessage = '신청 완료: ${_selectedDate.toLocal()} ${_selectedTime.format(context)}';
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('날짜 및 시간 선택'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            Text(
              '선택된 날짜: ${_selectedDate.toLocal()}',
              style: TextStyle(fontSize: 16),
            ),
            ElevatedButton(
              onPressed: () => _selectDate(context),
              child: Text('날짜 선택하기'),
            ),
            SizedBox(height: 16),
            Text(
              '선택된 시간: ${_selectedTime.format(context)}',
              style: TextStyle(fontSize: 16),
            ),
            ElevatedButton(
              onPressed: () => _selectTime(context),
              child: Text('시간 선택하기'),
            ),
            SizedBox(height: 24),
            ElevatedButton(
              onPressed: _confirmSelection,
              child: Text('확인'),
            ),
            SizedBox(height: 16),
            if (_confirmationMessage.isNotEmpty)
              Text(
                _confirmationMessage,
                style: TextStyle(fontSize: 16, color: Colors.green),
              ),
          ],
        ),
      ),
    );
  }
}
