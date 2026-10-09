# b-i-t-p-void timKiemHocSinh(const vector<HocSinh>& ds) {
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
}nh-m-A2
