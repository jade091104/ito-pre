import { useState } from 'react';
import {
  View,
  Text,
  TextInput,
  TouchableOpacity,
  FlatList,
  StyleSheet,
  Image,
  ScrollView
} from 'react-native';
import { Ionicons } from "@expo/vector-icons";

const App = () => {
  const [studentName, setStudentName] = useState('');
  const [studentYearSec, setStudentYearSec] = useState('');
  const [studentCourse, setStudentCourse] = useState('');
  const [studentRegister, setStudentRegister] = useState([]);
  const [editingIndex, setEditingIndex] = useState(null);
  const [currentScreen, setCurrentScreen] = useState('main');
  const [selectedItem, setSelectedItem] = useState(null);

  const handleSubmitStudent = () => {
    if (!studentName.trim() || !studentYearSec.trim() || !studentCourse.trim()) return;

    if (editingIndex !== null) {
      const updatedStudents = [...studentRegister];
      updatedStudents[editingIndex] = {
        text1: studentName,
        text2: studentYearSec,
        text3: studentCourse,
        id: updatedStudents[editingIndex].id,
      };
      setStudentRegister(updatedStudents);
      setEditingIndex(null);
    } else {
      setStudentRegister((currentStudents) => [
        ...currentStudents,
        {
          text1: studentName,
          text2: studentYearSec,
          text3: studentCourse,
          id: Math.random().toString(),
        },
      ]);
    }
    setStudentName('');
    setStudentYearSec('');
    setStudentCourse('');
  };

  const handleDeleteStudent = (index) => {
    const updatedStudents = [...studentRegister];
    updatedStudents.splice(index, 1);
    setStudentRegister(updatedStudents);
  };

  const startEditing = (index) => {
    setStudentName(studentRegister[index].text1);
    setStudentYearSec(studentRegister[index].text2);
    setStudentCourse(studentRegister[index].text3);
    setEditingIndex(index);
  };

  const handleClickItem = (item) => {
    setSelectedItem(item);
    setCurrentScreen('detail');
  };

  const goBack = () => {
    setSelectedItem(null);
    setCurrentScreen('main');
  };

  return (
    <View style={styles.container}>
      {currentScreen === 'main' ? (
        <View>
          <Text style={styles.title}>Student Management</Text>
          <View style={styles.inputContainer}>
            <Text>Name</Text>
            <TextInput
              placeholder="Enter Name"
              onChangeText={setStudentName}
              value={studentName}
              style={styles.textInput}
            />
            <Text>Year/Section</Text>
            <TextInput
              placeholder="Enter Year/Section"
              onChangeText={setStudentYearSec}
              value={studentYearSec}
              style={styles.textInput}
            />
            <Text>Course</Text>
            <TextInput
              placeholder="Enter Course"
              onChangeText={setStudentCourse}
              value={studentCourse}
              style={styles.textInput}
            />
            <TouchableOpacity onPress={handleSubmitStudent} style={styles.submitBtn}>
              <Text>{editingIndex !== null ? 'Update Record' : 'Add Record'}</Text>
            </TouchableOpacity>
          </View>

          <FlatList
            data={studentRegister}
            keyExtractor={(item) => item.id}
            renderItem={({ item, index }) => (
              <View style={styles.recordStudent}>
                <TouchableOpacity onPress={() => handleClickItem(item)}>
                  <Text style={styles.studentName}>{item.text1}</Text>
                  <View style={styles.studentDetails}>
                    <Text style={styles.grayText}>{item.text2}</Text>
                    <Text>-</Text>
                    <Text style={styles.grayText}>{item.text3}</Text>
                  </View>
                </TouchableOpacity>
                <View style={styles.actionButtons}>
                  <TouchableOpacity onPress={() => startEditing(index)}>
                    <Ionicons name="pencil-outline" size={20} />
                  </TouchableOpacity>
                  <TouchableOpacity onPress={() => handleDeleteStudent(index)} style={{ marginLeft: 10 }}>
                    <Ionicons name="trash" size={20} />
                  </TouchableOpacity>
                </View>
              </View>
            )}
          />
        </View>
      ) : (
        <View>
          <TouchableOpacity onPress={goBack}>
            <Text>Back</Text>
          </TouchableOpacity>
          <Image
            source={{ uri: 'https://cdna.artstation.com/p/assets/images/images/032/893/914/large/graham-w-sketch-80.jpg' }}
            style={styles.image}
          />
          <Text>{selectedItem ? selectedItem.text1 : 'None'}</Text>
          <Text>{selectedItem ? selectedItem.text2 : 'None'}</Text>
          <Text>{selectedItem ? selectedItem.text3 : 'None'}</Text>
        </View>
      )}
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    padding: 20,
  },
  title: {
    fontWeight: 'bold',
    fontSize: 22,
  },
  textInput: {
    borderWidth: 1,
    marginVertical: 10,
    padding: 10,
    textAlign: 'center',
  },
  submitBtn: {
    borderWidth: 1,
    padding: 8,
    width: '100%',
    alignItems: 'center',
  },
  recordStudent: {
    marginVertical: 10,
    padding: 10,
    borderWidth: 1,
    borderRadius: 5,
  },
  studentName: {
    fontSize: 14,
    fontWeight: 'bold',
  },
  studentDetails: {
    flexDirection: 'row',
    gap: 10,
  },
  grayText: {
    color: 'gray',
  },
  actionButtons: {
    flexDirection: 'row',
    gap: 10,
    marginTop: 5,
  },
  image: {
    width: '100%',
    height: 300,
  },
});

export default App;
