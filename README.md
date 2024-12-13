import sys
from PyQt5.QtWidgets import (
    QApplication, QMainWindow, QWidget, QLabel, QLineEdit, QPushButton, QVBoxLayout, QStackedWidget, QGridLayout, QComboBox,
    QCheckBox, QMessageBox
)
from PyQt5.QtCore import Qt


class LoginScreen(QWidget):
    def __init__(self, switch_to_main):
        super().__init__()
        self.switch_to_main = switch_to_main

        self.init_ui()

    def init_ui(self):
        layout = QVBoxLayout()

        self.username_label = QLabel("Username:")
        self.username_input = QLineEdit()
        self.password_label = QLabel("Password:")
        self.password_input = QLineEdit()
        self.password_input.setEchoMode(QLineEdit.Password)
        self.login_button = QPushButton("Login")
        self.login_button.clicked.connect(self.handle_login)

        layout.addWidget(self.username_label)
        layout.addWidget(self.username_input)
        layout.addWidget(self.password_label)
        layout.addWidget(self.password_input)
        layout.addWidget(self.login_button)

        self.setLayout(layout)

    def handle_login(self):
        username = self.username_input.text()
        password = self.password_input.text()

        if username == "admin" and password == "password":  # Placeholder credentials
            self.switch_to_main()
        else:
            QMessageBox.warning(self, "Login Failed", "Invalid username or password.")


class MainScreen(QMainWindow):
    def __init__(self):
        super().__init__()

        self.setWindowTitle("Network VT")
        self.setGeometry(100, 100, 800, 600)

        self.init_ui()

    def init_ui(self):
        layout = QGridLayout()

        self.dashboard_label = QLabel("Welcome to Kube Media Dashboard", self)
        self.dashboard_label.setAlignment(Qt.AlignCenter)

        self.studio_button = QPushButton("Studio Inbox")
        self.publisher_button = QPushButton("Social Publisher")
        self.scripts_button = QPushButton("Show Scripts")
        self.admin_button = QPushButton("Admin")
        self.logout_button = QPushButton("Logout")
        self.logout_button.clicked.connect(self.handle_logout)

        layout.addWidget(self.dashboard_label, 0, 0, 1, 2)
        layout.addWidget(self.studio_button, 1, 0)
        layout.addWidget(self.publisher_button, 1, 1)
        layout.addWidget(self.scripts_button, 2, 0)
        layout.addWidget(self.admin_button, 2, 1)
        layout.addWidget(self.logout_button, 3, 0, 1, 2)

        central_widget = QWidget()
        central_widget.setLayout(layout)
        self.setCentralWidget(central_widget)

    def handle_logout(self):
        self.parent().setCurrentIndex(0)


class NetworkVTApp(QStackedWidget):
    def __init__(self):
        super().__init__()

        self.login_screen = LoginScreen(self.show_main_screen)
        self.main_screen = MainScreen()

        self.addWidget(self.login_screen)
        self.addWidget(self.main_screen)

        self.setCurrentWidget(self.login_screen)

    def show_main_screen(self):
        self.setCurrentWidget(self.main_screen)


if __name__ == "__main__":
    app = QApplication(sys.argv)

    network_vt_app = NetworkVTApp()
    network_vt_app.setWindowTitle("Network VT Application")
    network_vt_app.resize(800, 600)
    network_vt_app.show()

    sys.exit(app.exec_())

