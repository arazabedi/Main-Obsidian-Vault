How to mock a function:

```js
import { describe, it, expect, vi, afterEach, beforeEach } from "vitest";
import User from "../../src/models/user.model.js";

describe("User model tests", () => {
  let mockUser;

  beforeEach(() => {
    vi.mock("../../src/models/user.model.js", () => {
      return {
        default: vi.fn().mockImplementation((params) => {
          return params;
        }),
      };
    });
  });

  afterEach(() => {
    vi.restoreAllMocks();
  });

  it("should create a new user with provided details", () => {
    const mockUserDetails = {
      username: "malcolmstrong",
      email: "malcolmstrong@hotmail.com",
      password: "123456",
    };

    mockUser = new User(mockUserDetails);

    expect(mockUser.username).toBe("malcolmstrong");
    expect(mockUser.email).toBe("malcolmstrong@hotmail.com");
    expect(mockUser.password).toBe("123456");
  });
});

```

Failed mock attempt
```js
      vi.mock("../../src/models/user.model.js", async (importOriginal) => {
        const testDate1 = new Date();
        const testDate2 = new Date(testDate1.getTime() + 60 * 60 * 24 * 1000);
        const testDate3 = new Date(testDate2.getTime() + 60 * 60 * 24 * 1000);
        const testDate4 = new Date(testDate3.getTime() + 60 * 60 * 24 * 1000);
        const weightLog = [
          { weight: 91.5, date: testDate1 },
          { weight: 91.4, date: testDate2 },
          { weight: 90.8, date: testDate3 },
          { weight: 91.2, date: testDate4 },
        ];

        const mockUserInstance = {
          weight_log: weightLog,
        };

        const mod = await importOriginal();
        return {
          __esModule: true,
          ...mod,
          default: {
            ...mod.default,
            findOne: vi.fn().mockResolvedValue(mockUserInstance),
          },
        };
      });

      const userId = "666a1aa8024c522d8a3692f3";
      const res = await User.findOne({ _id: userId });
      console.log("res: " + res);

      // Example of how you might test getWeightLog

      // const result = await getWeightLog(userId);
      // expect(User.findOne).toHaveBeenCalled();
      // expect(User.findOne).toHaveBeenCalledWith({ _id: userId });
      // expect(result).toEqual(weightLog);
```

```js
import { describe, it, expect, afterEach, vi, beforeEach } from "vitest";
import {
  logWeight,
  getWeightLog,
  addFriend,
  deleteFriend,
  getAllFriends,
  getAllFriendWeightLogs,
  getNameFromId,
} from "../../src/services/user.service.js";
import User from "../../src/models/user.model.js";

describe("User service tests", () => {
  afterEach(() => {
    vi.restoreAllMocks();
  });
  beforeEach(() => {
    vi.clearAllMocks();
    vi.mock("./../../src/models/user.model.js", () => {
      return {
        default: vi.fn(),
      };
    });
  });

  describe("logWeight", () => {
    it("should add a log object to the weight log", async () => {
      const user = {
        username: "malcolmstrong",
        email: "malcolmstrong@hotmail.com",
        password: "123456",
        weight_log: [],
        save: vi.fn(),
      };
      const logObject = {
        weight: 93.7,
        date: new Date(),
      };
      await logWeight(user, logObject);
      expect(user.weight_log).toContain(logObject);
      expect(user.save).toBeCalled();
    });
    it("should not save a log object with the same day as a previous log", async () => {
      const testDate = new Date();
      const user = {
        username: "malcolmstrong",
        email: "malcolmstrong@hotmail.com",
        password: "123456",
        weight_log: [{ weight: 90.5, date: testDate }],
        save: vi.fn(),
      };
      const logObject = {
        weight: 90.5,
        date: testDate,
      };
      await logWeight(user, logObject);
      expect(user.save).not.toBeCalled();
    });
    it("should add a log object with a next day date as a previous log", async () => {
      const testDate = new Date();
      const testDate2 = new Date(testDate.getTime() + 60 * 60 * 24 * 1000);
      const user = {
        username: "malcolmstrong",
        email: "malcolmstrong@hotmail.com",
        password: "123456",
        weight_log: [{ weight: 90.5, date: testDate }],
        save: vi.fn(),
      };
      const logObject = {
        weight: 90.5,
        date: testDate2,
      };
      await logWeight(user, logObject);
      expect(user.weight_log).toContain(logObject);
      expect(user.save).toBeCalled();
    });
    it("should throw an error when no user is found", async () => {
      const mockExec = vi.fn().mockResolvedValue(null);
      User.findOne.mockReturnValue({ exec: mockExec });

      await expect(() => logWeight()).rejects.toThrowError();
    });
  });

  // describe("getWeightLog", () => {
  //   it("should return the weight log for the given user id", async () => {
  //     const mockUser = {
  //       friends: ["friend1", "friend2"],
  //     };
  //     const mockExec = vi.fn().mockResolvedValue(mockUser);
  //     User.findOne.mockReturnValue({ exec: mockExec });

  //     const testDate = new Date();
  //     const userId = "666a1aa8024c522d8a3692f3";
  //     const friendLogs = await getAllFriendWeightLogs(userId);

  //     expect(friendLog[0]).toStrictEqual({
  //       friend_id: "friend1",
  //       friend_name: {
  //         first_Name: "Araz",
  //         middle_name: "Ashkan",
  //         last_name: "Abedi",
  //       },
  //       weightLog: [{ weight: 92.2, date: testDate }],
  //     });
  //   });
	// });

  // describe("getNameFromId", async () => {
  //   it("should return the full name given a user id", async () => {});
  //   const mockUser = {
  //     full_name: {
  //       first_name: "Malcolm",
  //       middle_name: "",
  //       last_name: "Strong",
  //     },
  //   };
  //   const mockExec = vi.fn().mockResolvedValue(mockUser);
  //   User.findOne.mockReturnValue({ exec: mockExec });

  //   const userId = "666a1aa8024c522d8a3692f3";

  //   const fullName = await getNameFromId(userId);
  //   expect(fullName).toStrictEqual({
  //     first_name: "Malcolm",
  //     middle_name: "",
  //     last_name: "Strong",
  //   });
  //   it("should throw an error if the userId is not found", async () => {
  //     const mockExec = vi.fn().mockResolvedValue(null);
  //     User.findOne.mockReturnValue({ exec: mockExec });
  //     await expect(getNameFromId()).rejects.toThrowError();
  //   });
  // });
});

```