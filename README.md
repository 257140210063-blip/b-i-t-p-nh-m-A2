#include <iostream>
#include <string>
#include <vector>
#include <iomanip>
#include <algorithm>

using namespace std;

class HocSinh {
private:
    string maHS;
    string hoTen;
    string lop;
    double diemToan;
    double diemVan;
    double diemAnh;
    double diemTB;
    string xepLoai;

public:
    HocSinh() {
        maHS = "";
        hoTen = "";
        lop = "";
        diemToan = diemVan = diemAnh = diemTB = 0.0;
        xepLoai = "Chua xep loai";
    }

    HocSinh(string ma, string ten, string l, double toan, double van, double anh) {
        maHS = ma;
        hoTen = ten;
        lop = l;
        diemToan = toan;
        diemVan = van;
        diemAnh = anh;
        capNhatKetQua();
    }
    void capNhatKetQua() {
        diemTB = (diemToan + diemVan + diemAnh) / 3.0;
        if (diemTB >= 8.0) xepLoai = "Gioi";
        else if (diemTB >= 6.5) xepLoai = "Kha";
        else if (diemTB >= 5.0) xepLoai = "Trung Binh";
        else xepLoai = "Yeu";
    }

    string getMaHS() const { return maHS; }
    string getHoTen() const { return hoTen; }
    double getDiemTB() const { return diemTB; }
    friend istream& operator>>(istream& is, HocSinh& hs);
    friend ostream& operator<<(ostream& os, const HocSinh& hs);
};

istream& operator>>(istream& is, HocSinh& hs) {
    cout << "  Nhap ma hoc sinh : ";
    getline(is >> ws, hs.maHS);
    cout << "  Nhap ho va ten   : ";
    getline(is, hs.hoTen);
    cout << "  Nhap lop         : ";
    getline(is, hs.lop);
    
    do {
        cout << "  Nhap diem Toan   : "; 
        is >> hs.diemToan;
        if (hs.diemToan < 0 || hs.diemToan > 10) 
            cout << "    [!] Diem phai nam trong khoang 0 - 10.\n";
    } while (hs.diemToan < 0 || hs.diemToan > 10);

    do {
        cout << "  Nhap diem Van    : "; 
        is >> hs.diemVan;
        if (hs.diemVan < 0 || hs.diemVan > 10) 
            cout << "    [!] Diem phai nam trong khoang 0 - 10.\n";
    } while (hs.diemVan < 0 || hs.diemVan > 10);

    do {
        cout << "  Nhap diem Anh    : "; 
        is >> hs.diemAnh;
        if (hs.diemAnh < 0 || hs.diemAnh > 10) 
            cout << "    [!] Diem phai nam trong khoang 0 - 10.\n";
    } while (hs.diemAnh < 0 || hs.diemAnh > 10);

    is.ignore(10000, '\n');

    hs.capNhatKetQua();
    return is;
}

ostream& operator<<(ostream& os, const HocSinh& hs) {
    os << left << setw(10) << hs.maHS
       << setw(22) << hs.hoTen
       << setw(10) << hs.lop
       << setw(8)  << fixed << setprecision(1) << hs.diemToan
       << setw(8)  << hs.diemVan
       << setw(8)  << hs.diemAnh
       << setw(8)  << setprecision(2) << hs.diemTB
       << setw(12) << hs.xepLoai;
    return os;
}

string toLower(string str) {
    transform(str.begin(), str.end(), str.begin(), ::tolower);
    return str;
}

void inTieuDe() {
    cout << string(86, '-') << endl;
    cout << left 
         << setw(10) << "Ma HS"
         << setw(22) << "Ho va Ten"
         << setw(10) << "Lop"
         << setw(8)  << "Toan"
         << setw(8)  << "Van"
         << setw(8)  << "Anh"
         << setw(8)  << "DTB"
         << setw(12) << "Xep Loai" << endl;
    cout << string(86, '-') << endl;
}

void inDanhSach(const vector<HocSinh>& ds) {
    if (ds.empty()) {
        cout << "\n[!] Danh sach hien dang rong!\n";
        return;
    }

    inTieuDe();

    for (const auto& hs : ds) {
        cout << hs << "\n"; 
    }


    cout << string(86, '-') << endl;
}
void sapXepGiamDanTheoDTB(vector<HocSinh>& ds) {
    if (ds.empty()) {
        cout << "\n[!] Danh sach rong, khong the sap xep!\n";
        return;
    }
    sort(ds.begin(), ds.end(), [](const HocSinh& a, const HocSinh& b) {
        return a.getDiemTB() > b.getDiemTB();
    });
    cout << "\n[=>] Da sap xep danh sach theo diem trung binh giam dan!\n";
}

void timKiemHocSinh(const vector<HocSinh>& ds) {
    if (ds.empty()) {
        cout << "\n[!] Danh sach rong, khong the tim kiem!\n";
        return;
    }
    cout << "Nhap Ma HS hoac Ho ten can tim: ";
    string tuKhoa;
    getline(cin >> ws, tuKhoa);
    tuKhoa = toLower(tuKhoa);

    bool timThay = false;
    for (const auto& hs : ds) {
        if (toLower(hs.getMaHS()).find(tuKhoa) != string::npos || 
            toLower(hs.getHoTen()).find(tuKhoa) != string::npos) {
            if (!timThay) {
                cout << "\n--- KET QUA TIM KIEM ---\n";
                inTieuDe();
                timThay = true;
            }
            cout << hs << "\n";
        }
    }
    if (!timThay) {
        cout << "Khong tim thay hoc sinh nao phu hop voi tu khoa: \"" << tuKhoa << "\"\n";
    } else {
        cout << string(86, '-') << endl;
    }
}

void boSungHocSinh(vector<HocSinh>& ds) {
    if (ds.size() >= 200) {
        cout << "\n[!] Danh sach da dat gioi han toi da (200 hoc sinh)!\n";
        return;
    }
    int pos;
    cout << "Nhap vi tri can chen (1 den " << ds.size() + 1 << "): ";
    cin >> pos;

    if (pos < 1 || pos > (int)ds.size() + 1) {
        cout << "[!] Vi tri khong hop le!\n";
        return;
    }

    HocSinh hsMoi;
    cout << "Nhap thong tin hoc sinh moi:\n";
    cin >> hsMoi;

    ds.insert(ds.begin() + (pos - 1), hsMoi);
    cout << "[=>] Da them hoc sinh vao vi tri " << pos << " thanh cong!\n";
}

void xoaHocSinh(vector<HocSinh>& ds) {
    if (ds.empty()) {
        cout << "\n[!] Danh sach rong, khong co gi de xoa!\n";
        return;
    }
    int pos;
    cout << "Nhap vi tri can xoa (1 den " << ds.size() << "): ";
    cin >> pos;

    if (pos < 1 || pos > (int)ds.size()) {
        cout << "[!] Vi tri khong hop le!\n";
        return;
    }

    ds.erase(ds.begin() + (pos - 1));
    cout << "[=>] Da xoa hoc sinh tai vi tri " << pos << " thanh cong!\n";
}

int main() {
    vector<HocSinh> dsHocSinh;
    int n;

 
    do {
        cout << "Nhap so luong hoc sinh ban dau (0 < n < 200): ";
        cin >> n;
        if (n <= 0 || n >= 200) {
            cout << "[!] So luong khong hop le. Vui long nhap lai!\n";
        }
    } while (n <= 0 || n >= 200);

    cout << "\n=== NHAP DANH SACH " << n << " HOC SINH ===\n";
    for (int i = 0; i < n; ++i) {
        cout << "\nHoc sinh thu " << i + 1 << ":\n";
        HocSinh hs;
        cin >> hs;
        dsHocSinh.push_back(hs);
    }

    int luachon;
    do {
        cout << "\n================ MENU QUAN LY HOC SINH ================\n";
        cout << "1. In danh sach hoc sinh\n";
        cout << "2. Sap xep danh sach theo DTB giam dan\n";
        cout << "3. Tim kiem hoc sinh theo Ma HS hoac Ho ten\n";
        cout << "4. Bo sung 1 hoc sinh vao vi tri cho truoc\n";
        cout << "5. Xoa 1 hoc sinh tai vi tri cho truoc\n";
        cout << "0. Thoat chuong trinh\n";
        cout << "=======================================================\n";
        cout << "Chon chuc nang (0-5): ";
        cin >> luachon;

        switch (luachon) {
            case 1:
                cout << "\n=== DANH SACH HOC SINH ===\n";
                inDanhSach(dsHocSinh);
                break;
            case 2:
                sapXepGiamDanTheoDTB(dsHocSinh);
                inDanhSach(dsHocSinh);
                break;
            case 3:
                timKiemHocSinh(dsHocSinh);
                break;
            case 4:
                boSungHocSinh(dsHocSinh);
                break;
            case 5:
                xoaHocSinh(dsHocSinh);
                break;
            case 0:
                cout << "Da thoat chuong trinh.\n";
                break;
            default:
                cout << "[!] Lua chon khong hop le. Vui long chon lai!\n";
        }
    } while (luachon != 0);

    return 0;
}

