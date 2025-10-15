# CPP-DSA
    #include <iostream>
    #include <vector>
    #include <string>
    #include <algorithm>


    class StudentManagement {
    
    private:
        struct Student {
          int id;
          std::string name;
          double gpa;
        };
        std::vector<Student> students;


    public:

        int partition(int low, int high) {
            std::string pivot = students[high].name;
            int i = low - 1;
            for (int j = low; j < high; j++) {
                if (students[j].name < pivot) {
                    i++;
                    std::swap(students[i], students[j]);
                }
            }
            std::swap(students[i + 1], students[high]);
            return i + 1;
        }
        
        void quickSort(int low, int high) {
            if (low < high) {
                int pi = partition(low, high);
                quickSort(low, pi - 1);
                quickSort(pi + 1, high);
            }
        }
        
        void mergeByGPA(int left, int mid, int right) {
            int s1 = mid - left + 1;
            int s2 = right - mid;
            std::vector<Student> L(s1), R(s2);
            
            for (int i = 0; i < s1; i++) L[i] = students[left + i];
            for (int i = 0; i < s2; i++) R[i] = students[mid + 1 + i];
    
            int i = 0, j = 0, k = left;
            while (i < s1 && j < s2) {
                if (L[i].gpa <= R[j].gpa) students[k++] = L[i++];
                else students[k++] = R[j++];
            }
            while (i < s1) students[k++] = L[i++];
            while (j < s2) students[k++] = R[j++];
        }
    
        void mergeSortByGPA(int left, int right) {
            if (left < right) {
                int mid = left + (right - left) / 2;
                mergeSortByGPA(left, mid);
                mergeSortByGPA(mid + 1, right);
                mergeByGPA(left, mid, right);
            }
        }
    
        void addStudent(int id, const std::string& name, double gpa) {
            students.push_back({id, name, gpa});
        }
    
        void deleteStudent(int id) {
            bool found = false;
            for (auto it = students.begin(); it != students.end(); ++it) {
                if (it->id == id) {
                    students.erase(it);
                    found = true;
                    break;
                }
            }
            if (!found) std::cout << "Student not found!\n";
        }
    
        void displayStudents() {
            for (const auto& s : students)
                std::cout << s.id << "-" << s.name << "-" << s.gpa << "\n";
        }
    
        void sortByName() {
            if (students.empty()) return;
            quickSort(0, students.size() - 1);
        }
    
        void sortByGPA() {
            if (students.empty()) return;
            mergeSortByGPA(0, students.size() - 1);
        }
    
        void sortByID() {
            std::sort(students.begin(), students.end(), [](const Student& a, const Student& b) {
                return a.id < b.id;
            });
        }
        
        int binarySearchByID(int id) {
            sortByID();
            int left = 0, right = students.size() - 1;
            while (left <= right) {
                int mid = left + (right - left) / 2;
                if (students[mid].id == id) return mid;
                else if (students[mid].id < id) left = mid + 1;
                else right = mid - 1;
            }
            return -1;
        }
    };

    int main() {

        StudentManagement uni;
        uni.addStudent(3002, "Siranush", 3.8);
        uni.addStudent(1024, "Davit", 3.5);
        uni.addStudent(1098, "Narek", 3.9);
        uni.addStudent(3540, "Dayana", 3.7);
    
        std::cout << "Students:\n";
        uni.displayStudents();
    
        uni.sortByID();
        uni.displayStudents();
    
    
        std::cout << "Sorting by Name:\n";
        uni.sortByName();
        uni.displayStudents();
    
        std::cout << "Sorting by GPA:\n";
        uni.sortByGPA();
        uni.displayStudents();
    
    
        int searchID = 1098;
        int index = uni.binarySearchByID(searchID);
        if (index != -1)
            std::cout << "Found student with ID " << searchID << " at index " << index << "\n";
        else
            std::cout << "Student not found!\n";
    
        std::cout << "Deleting student\n";
        uni.deleteStudent(1098);
        uni.displayStudents();
    
        return 0;
    }

